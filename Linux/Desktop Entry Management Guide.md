# Desktop Entry Management Guide

A consolidated reference for maintaining, cleaning, and automating custom `.desktop` file overrides in Arch Linux / EndeavourOS.

---

## 1. Overview & Goals

1. **User Overrides**: Copy launchers from `/usr/share/applications/` to `~/.local/share/applications/` so modifications persist across package updates.
2. **Localization Pruning**: Remove redundant non-English localized entries (`Name[...]`, `Comment[...]`, `Keywords[...]`), preserving base values and `[en_GB]`.
3. **Startup Notification**: Force `StartupNotify=false` across all desktop launchers.
4. **Validation & Database Refresh**: Automate MIME database updates and filter out noisy upstream validator warnings.

---

## 2. Maintenance Script (`sync-desktop-overrides`)

Save this script to `~/.local/bin/sync-desktop-overrides`:

```bash
#!/usr/bin/env bash
set -euo pipefail

TARGET_DIR="$HOME/.local/share/applications"
SOURCE_DIR="/usr/share/applications"
DRY_RUN=0

# Parse arguments
for arg in "$@"; do
  case "$arg" in
    -n|--dry-run)
      DRY_RUN=1
      echo "[DRY-RUN MODE] No changes will be written."
      ;;
  esac
done

mkdir -p "$TARGET_DIR"

echo "==> Checking for new desktop files from $SOURCE_DIR..."
for src in "$SOURCE_DIR"/*.desktop; do
  [ -f "$src" ] || continue
  dest="$TARGET_DIR/$(basename "$src")"
  if [ ! -f "$dest" ]; then
    if [ "$DRY_RUN" -eq 1 ]; then
      echo "[DRY-RUN] Would copy: $src -> $dest"
    else
      cp "$src" "$dest"
    fi
  fi
done

echo "==> Scanning and sanitizing launcher entries in $TARGET_DIR..."
for f in "$TARGET_DIR"/*.desktop; do
  [ -f "$f" ] || continue

  # 1. Strip localized Name/Comment/Keywords except [en_GB]
  if grep -E -q '^(Name|Comment|Keywords)\[[^]]+\]' "$f" && grep -E '^(Name|Comment|Keywords)\[[^]]+\]' "$f" | grep -qv '\[en_GB\]'; then
    if [ "$DRY_RUN" -eq 1 ]; then
      echo "[DRY-RUN] Would strip localized keys in: $f"
    else
      sed -i -E '/^(Name|Comment|Keywords)\[/ { /^(Name|Comment|Keywords)\[en_GB\]/!d }' "$f"
    fi
  fi

  # 2. Replace StartupNotify=true with StartupNotify=false
  if grep -q '^StartupNotify=true$' "$f"; then
    if [ "$DRY_RUN" -eq 1 ]; then
      echo "[DRY-RUN] Would set StartupNotify=false in: $f"
    else
      sed -i 's/^StartupNotify=true$/StartupNotify=false/' "$f"
    fi
  fi

  # 3. Append StartupNotify=false under [Desktop Entry] if missing
  if ! grep -q '^StartupNotify=' "$f"; then
    if [ "$DRY_RUN" -eq 1 ]; then
      echo "[DRY-RUN] Would append StartupNotify=false in: $f"
    else
      sed -i '/^\[Desktop Entry\]/a StartupNotify=false' "$f"
    fi
  fi

  # 4. Remove deprecated Encoding= keys
  if grep -q '^Encoding=' "$f"; then
    if [ "$DRY_RUN" -eq 1 ]; then
      echo "[DRY-RUN] Would remove deprecated Encoding key in: $f"
    else
      sed -i '/^Encoding=/d' "$f"
    fi
  fi
done

if [ "$DRY_RUN" -eq 0 ]; then
  echo "==> Updating desktop launcher database..."
  update-desktop-database "$TARGET_DIR"
  echo "==> Synchronization complete."
else
  echo "==> Dry run complete. Database update skipped."
fi
```

Make it executable:

```bash
chmod +x ~/.local/bin/sync-desktop-overrides
```

---

## 3. Automated Pacman Hook

Automatically re-sync and sanitize whenever Pacman installs or updates application packages:

Create `/etc/pacman.d/hooks/95-sync-desktop-overrides.hook`:

```ini
[Trigger]
Type = Path
Operation = Install
Operation = Upgrade
Target = usr/share/applications/*.desktop

[Action]
Description = Synchronizing and sanitizing desktop launcher overrides...
When = PostTransaction
Exec = /usr/bin/su - lee -c "/home/lee/.local/bin/sync-desktop-overrides"
```

---

## 4. Shell Validation Helpers (`~/.bashrc`)

Add this validation function or alias to `~/.bashrc`. It uses `rg` to filter out harmless upstream warnings (redundant keywords and category hints):

```bash
# Validate desktop entries without noise
validate-desktop() {
  local files=("$@")
  if [ ${#files[@]} -eq 0 ]; then
    files=("$HOME/.local/share/applications"/*.desktop)
  fi
  desktop-file-validate "${files[@]}" 2>&1 | rg -v '(contains keyword|hint:)'
}

alias check-desktop='desktop-file-validate "$HOME/.local/share/applications"/*.desktop 2>&1 | rg -v "(contains keyword|hint:)"'
```

Reload config:

```bash
source ~/.bashrc
```

---

## 5. Maintenance Commands

### Dry Run Sync
```bash
~/.local/bin/sync-desktop-overrides --dry-run
```

### Full Run Sync & Rebuild
```bash
~/.local/bin/sync-desktop-overrides
```

### Remove Orphaned Overrides
Removes overrides in `~/.local/share/applications/` whose original package in `/usr/share/applications/` was uninstalled:

```bash
for f in "$HOME/.local/share/applications"/*.desktop; do
  [ -f "$f" ] || continue
  sys_file="/usr/share/applications/$(basename "$f")"
  if [ ! -f "$sys_file" ]; then
    echo "Removing orphan: $f"
    rm "$f"
  fi
done
update-desktop-database "$HOME/.local/share/applications"
```

---

## 6. Validator Issues Reference

| Diagnostic | Cause | Fix / Resolution |
| :--- | :--- | :--- |
| `warning: key "Encoding" ... is deprecated` | Obsolete `Encoding=UTF-8` key from legacy specifications. | Handled automatically by the script (`sed -i '/^Encoding=/d'`). |
| `warning: value ... contains keyword X which is the same as ... Name` | Upstream package includes redundant keywords already indexed by desktop searches. | Benign. Filtered out by `rg -v 'contains keyword'`. |
| `hint: value ... contains more than one main category` | Application assigned multiple primary categories (e.g. `Office;Utility;`). | Harmless hint. Filtered out by `rg -v 'hint:'`. |
| `error: value item "Video" ... requires ... AudioVideo` | Spec requires `AudioVideo` whenever `Video` is specified. | Add `AudioVideo;` to `Categories=` in the affected file (e.g., `Tapo-Camera.desktop`). |
| `rg: error parsing flag -E` | Shell aliases `grep` to `rg`, where `-E` means `--encoding` instead of regex. | Use `rg` directly without `-E`, or run `\grep` to bypass shell aliases. |