File compression methods linux

  If the folder or file name has spaces, you need to quote or escape it, otherwise
tar will think it’s multiple arguments.


HOW TO COMPRESS

--------------------------------------------------------------------------------


1. USING QUOTES

tar -cvf "Wentworth Castle.tar" "Wentworth Castle/"

--------------------------------------------------------------------------------


2. ESCAPING SPACES WITH \

tar -cvf Wentworth\ Castle\ Gardens.tar Wentworth\ Castle\ 2021/

--------------------------------------------------------------------------------


3. WITH COMPRESSION (SMALLER ARCHIVE)

 * With gzip (fast, decent):
   
   tar -cvzf "Wentworth Castle.tar.gz" "Wentworth Castle/"

 * With xz (slower, smaller):
   
   tar -cvJf "Wentworth Castle.tar.xz" "Wentworth Castle/"

 * With zstd (modern, fast + small):
   
   tar -I 'zstd -19' -cvf "Wentworth Castle.tar.zst" "Wentworth Castle/"

--------------------------------------------------------------------------------




HOW TO EXTRACT



--------------------------------------------------------------------------------


1. IF YOU MADE A PLAIN .TAR

tar -xvf "Wentworth Castle.tar"

--------------------------------------------------------------------------------


2. IF YOU MADE A .TAR.GZ

tar -xvzf "Wentworth Castle.tar.gz"

--------------------------------------------------------------------------------


3. IF YOU MADE A .TAR.XZ

tar -xvJf "Wentworth Castle.tar.xz"

--------------------------------------------------------------------------------


4. IF YOU MADE A .TAR.ZST (ZSTD)

tar --zstd -xvf "Wentworth Castle.tar.zst"

--------------------------------------------------------------------------------


SUMMARY

✅ General rule:
 * c = create,
 * x = extract,
 * v = verbose (shows filenames, can omit),
 * f = file (archive name next).

So:
 * tar -cvf → create archive
 * tar -xvf → extract archive

--------------------------------------------------------------------------------




TEST /EXPLORE INSIDE THE ARCHIVE WITHOUT EXTRACTING

Do you want me to also show you how to test/explore inside the archive without
fully extracting it? (handy for checking backups).

Great 👍 Here’s how you can peek inside a tar archive without extracting:

--------------------------------------------------------------------------------


🗂️ LIST CONTENTS

tar -tvf "Wentworth Castle.tar"

 * t = list (table of contents)
 * v = verbose (shows details like size, dates)
 * f = file

👉 Works with .tar.gz, .tar.xz, .tar.zst too — just add the right compression
option if needed:
 * .tar.gz → tar -tvzf "Wentworth Castle Gardens 2021.tar.gz"
 * .tar.xz → tar -tvJf "Wentworth Castle Gardens 2021.tar.xz"
 * .tar.zst → tar --zstd -tvf "Wentworth Castle Gardens 2021.tar.zst"

--------------------------------------------------------------------------------


🔎 VIEW A SINGLE FILE INSIDE

tar -xvf "Wentworth Castle.tar" "Wentworth Castle/photo001.jpg"

That extracts just one file (instead of everything).

--------------------------------------------------------------------------------


✅ TEST ARCHIVE INTEGRITY (WITHOUT EXTRACTING)

For gzip/xz/zstd, you can test like this:
 * .tar.gz:
   
   gzip -t "Wentworth Castle.tar.gz"

 * .tar.xz:
   
   xz -t "Wentworth Castle.tar.xz"

 * .tar.zst:
   
   zstd -t "Wentworth Castle.tar.zst"

--------------------------------------------------------------------------------




SAFE ONE LINE TEST

Nice 👌 here’s a safe one-liner method: it will create the archive and then
immediately test it before you delete or move the originals.

--------------------------------------------------------------------------------


🔒 WITH GZIP

tar -cvzf "Wentworth Castle.tar.gz" "Wentworth Castle/" &amp;&amp; gzip -t "Wentworth Castle.tar.gz"

--------------------------------------------------------------------------------


🔒 WITH XZ

tar -cvJf "Wentworth Castle.tar.xz" "Wentworth Castle/" &amp;&amp; xz -t "Wentworth Castle.tar.xz"

--------------------------------------------------------------------------------


🔒 WITH ZSTD

tar -I 'zstd -19' -cvf "Wentworth Castle.tar.zst" "Wentworth Castle/" &amp;&amp; zstd -t "Wentworth Castletar.zst"

--------------------------------------------------------------------------------

✅ If the test succeeds, it will just return to the shell without errors.
❌ If there’s a problem, you’ll see an error message (so you know not to trust
the archive).

--------------------------------------------------------------------------------


COMPARE FILES IN ARCHIVE AGAINST THE ORIGINALS TO CHECK NOTHING IS MISSING?

Nice 👌 here’s a safe one-liner method: it will create the archive and then
immediately test it before you delete or move the originals.

--------------------------------------------------------------------------------


🔒 WITH GZIP

tar -cvzf "Wentworth Castle.tar.gz" "Wentworth Castle/" &amp;&amp; gzip -t "Wentworth Castle.tar.gz"

--------------------------------------------------------------------------------


🔒 WITH XZ

tar -cvJf "Wentworth Castle.tar.xz" "Wentworth Castle/" &amp;&amp; xz -t "Wentworth Castle.tar.xz"

--------------------------------------------------------------------------------


🔒 WITH ZSTD

tar -I 'zstd -19' -cvf "Wentworth Castle.tar.zst" "Wentworth Castle/" &amp;&amp; zstd -t "Wentworth Castle.tar.zst"

--------------------------------------------------------------------------------

✅ If the test succeeds, it will just return to the shell without errors.
❌ If there’s a problem, you’ll see an error message (so you know not to trust
the archive).

--------------------------------------------------------------------------------

Would you like me to also add a step that compares the file list inside the
archive vs. the original folder to double-check nothing is missing?



🎉 Awesome — here’s a clean backup script you can drop into a file (e.g.
backup.sh) and run:

#!/bin/bash
# Backup script: compress, verify, and compare file lists
# Usage: ./backup.sh "Folder Name With Spaces"

set -euo pipefail

# Input folder
FOLDER="$1"

# Output archive name (replace spaces with underscores)
ARCHIVE="${FOLDER// /_}.tar.xz"

echo "📦 Creating archive: $ARCHIVE"
tar -cvJf "$ARCHIVE" "$FOLDER"

echo "🔍 Testing archive integrity..."
xz -t "$ARCHIVE"

echo "📋 Comparing file lists..."
diff -u \
  &lt;(find "$FOLDER" -type f | sort) \
  &lt;(tar -tvJf "$ARCHIVE" | awk '{print $6}' | sort)

if [ $? -eq 0 ]; then
    echo "✅ Backup verified: $ARCHIVE"
else
    echo "❌ Differences found! Check the lists above."
fi


--------------------------------------------------------------------------------


🔧 HOW TO USE IT

 1. Save as backup.sh
 2. Make it executable:
    
    chmod +x backup.sh

 3. Run it with your folder:
    
    ./backup.sh "Wentworth Castle"

--------------------------------------------------------------------------------

✅ This will:
 1. Create a .tar.xz archive
 2. Test archive integrity
 3. Compare file list inside vs original folder
 4. Print ✅ Backup verified if all matches

--------------------------------------------------------------------------------

Would you like me to also add a step that automatically checks the archive size
vs the original folder size (so you get a neat summary of space saved)?



Nice 🚀 Let’s make it even more useful: here’s the improved backup script with a
size summary showing how much space you saved.

#!/bin/bash
# Backup script: compress, verify, compare file lists, and report size savings
# Usage: ./backup.sh "Folder Name With Spaces"

set -euo pipefail

# Input folder
FOLDER="$1"

# Output archive name (replace spaces with underscores)
ARCHIVE="${FOLDER// /_}.tar.xz"

echo "📦 Creating archive: $ARCHIVE"
tar -cvJf "$ARCHIVE" "$FOLDER"

echo "🔍 Testing archive integrity..."
xz -t "$ARCHIVE"

echo "📋 Comparing file lists..."
diff -u \
  &lt;(find "$FOLDER" -type f | sort) \
  &lt;(tar -tvJf "$ARCHIVE" | awk '{print $6}' | sort)

if [ $? -eq 0 ]; then
    echo "✅ Backup verified: $ARCHIVE"
else
    echo "❌ Differences found! Check the lists above."
    exit 1
fi

# --- Size check ---
echo "📊 Checking size savings..."
FOLDER_SIZE=$(du -sh "$FOLDER" | cut -f1)
ARCHIVE_SIZE=$(du -sh "$ARCHIVE" | cut -f1)

FOLDER_BYTES=$(du -sb "$FOLDER" | cut -f1)
ARCHIVE_BYTES=$(du -sb "$ARCHIVE" | cut -f1)

SAVED=$((FOLDER_BYTES - ARCHIVE_BYTES))
PERCENT=$((100 * SAVED / FOLDER_BYTES))

echo "📂 Original folder: $FOLDER_SIZE"
echo "🗜️  Archive size:  $ARCHIVE_SIZE"
echo "💾 Space saved:   $((SAVED / 1024 / 1024)) MB (~$PERCENT%)"


--------------------------------------------------------------------------------


🛠️ WHAT’S NEW

 * Reports the original folder size and the archive size
 * Shows how much space was saved in MB and %

--------------------------------------------------------------------------------

Example output after running:

📦 Creating archive: Wentworth_Castle.tar.xz
🔍 Testing archive integrity...
📋 Comparing file lists...
✅ Backup verified: Wentworth_Castle.tar.xz
📊 Checking size savings...
📂 Original folder: 600M
🗜️  Archive size:  420M
💾 Space saved:   180 MB (~30%)

--------------------------------------------------------------------------------




AUTO MOVES THE ARCHIVE

👉 Do you also want me to extend it so it automatically moves the archive to a
backup location (e.g. /mnt/backup/) after success?



🎉 Boom — let’s make it a one-stop backup tool that also moves the archive into
a backup location after everything checks out.

Here’s the improved script:

#!/bin/bash
# Backup script: compress, verify, compare file lists, report size savings, and move archive
# Usage: ./backup.sh "Folder Name With Spaces" /path/to/backup/location

set -euo pipefail

# Input arguments
FOLDER="$1"
BACKUP_DIR="${2:-$HOME/backups}"   # default backup location if not given

# Ensure backup directory exists
mkdir -p "$BACKUP_DIR"

# Output archive name (replace spaces with underscores)
ARCHIVE="${FOLDER// /_}.tar.xz"
ARCHIVE_PATH="$BACKUP_DIR/$ARCHIVE"

echo "📦 Creating archive: $ARCHIVE_PATH"
tar -cvJf "$ARCHIVE_PATH" "$FOLDER"

echo "🔍 Testing archive integrity..."
xz -t "$ARCHIVE_PATH"

echo "📋 Comparing file lists..."
diff -u \
  &lt;(find "$FOLDER" -type f | sort) \
  &lt;(tar -tvJf "$ARCHIVE_PATH" | awk '{print $6}' | sort)

if [ $? -eq 0 ]; then
    echo "✅ Backup verified: $ARCHIVE_PATH"
else
    echo "❌ Differences found! Check the lists above."
    exit 1
fi

# --- Size check ---
echo "📊 Checking size savings..."
FOLDER_SIZE=$(du -sh "$FOLDER" | cut -f1)
ARCHIVE_SIZE=$(du -sh "$ARCHIVE_PATH" | cut -f1)

FOLDER_BYTES=$(du -sb "$FOLDER" | cut -f1)
ARCHIVE_BYTES=$(du -sb "$ARCHIVE_PATH" | cut -f1)

SAVED=$((FOLDER_BYTES - ARCHIVE_BYTES))
PERCENT=$((100 * SAVED / FOLDER_BYTES))

echo "📂 Original folder: $FOLDER_SIZE"
echo "🗜️  Archive size:  $ARCHIVE_SIZE"
echo "💾 Space saved:   $((SAVED / 1024 / 1024)) MB (~$PERCENT%)"

echo "📁 Archive stored at: $ARCHIVE_PATH"


--------------------------------------------------------------------------------


🛠️ HOW TO USE

 1. Save it as backup.sh
 2. Make it executable:
    
    chmod +x backup.sh

 3. Run it:
    * With a custom backup directory:
      
      ./backup.sh "Wentworth Castle" /mnt/backup
    
    * Or just let it default to ~/backups:
      
      ./backup.sh "Wentworth Castle"

--------------------------------------------------------------------------------

✅ Features now:
 * Creates .tar.xz archive
 * Verifies archive integrity
 * Compares file lists
 * Reports size savings
 * Moves the archive to your backup folder (~/backups by default or a custom one
   you choose)

--------------------------------------------------------------------------------




AUTO TIMESTAMP ARCHIVE NAME

Would you like me to also add a feature so the script can automatically
timestamp the archive name (e.g. Wentworth_Castle_Gardens.tar.xz) for versioned
backups?

💯 Nice — versioned backups are the icing on the cake. With timestamps, you’ll
never overwrite an old backup by accident.

Here’s the final boss script 🏆 with timestamps added:

#!/bin/bash
# Backup script: compress, verify, compare file lists, report size savings, and move archive with timestamp
# Usage: ./backup.sh "Folder Name With Spaces" /path/to/backup/location

set -euo pipefail

# Input arguments
FOLDER="$1"
BACKUP_DIR="${2:-$HOME/backups}"   # default backup location if not given

# Ensure backup directory exists
mkdir -p "$BACKUP_DIR"

# Add timestamp (YYYY-MM-DD)
DATESTAMP=$(date +%F)

# Output archive name (replace spaces with underscores + timestamp)
BASENAME="${FOLDER// /_}"
ARCHIVE="${BASENAME}_${DATESTAMP}.tar.xz"
ARCHIVE_PATH="$BACKUP_DIR/$ARCHIVE"

echo "📦 Creating archive: $ARCHIVE_PATH"
tar -cvJf "$ARCHIVE_PATH" "$FOLDER"

echo "🔍 Testing archive integrity..."
xz -t "$ARCHIVE_PATH"

echo "📋 Comparing file lists..."
diff -u \
  &lt;(find "$FOLDER" -type f | sort) \
  &lt;(tar -tvJf "$ARCHIVE_PATH" | awk '{print $6}' | sort)

if [ $? -eq 0 ]; then
    echo "✅ Backup verified: $ARCHIVE_PATH"
else
    echo "❌ Differences found! Check the lists above."
    exit 1
fi

# --- Size check ---
echo "📊 Checking size savings..."
FOLDER_SIZE=$(du -sh "$FOLDER" | cut -f1)
ARCHIVE_SIZE=$(du -sh "$ARCHIVE_PATH" | cut -f1)

FOLDER_BYTES=$(du -sb "$FOLDER" | cut -f1)
ARCHIVE_BYTES=$(du -sb "$ARCHIVE_PATH" | cut -f1)

SAVED=$((FOLDER_BYTES - ARCHIVE_BYTES))
PERCENT=$((100 * SAVED / FOLDER_BYTES))

echo "📂 Original folder: $FOLDER_SIZE"
echo "🗜️  Archive size:  $ARCHIVE_SIZE"
echo "💾 Space saved:   $((SAVED / 1024 / 1024)) MB (~$PERCENT%)"

echo "📁 Archive stored at: $ARCHIVE_PATH"


--------------------------------------------------------------------------------


EXAMPLE RUN

./backup.sh "Wentworth Castle" /mnt/backup

Creates:

/mnt/backup/Wentworth_Castle_2025-08-23.tar.xz

--------------------------------------------------------------------------------

🔥 Now every run makes a dated archive, so you can keep a history of backups
without overwriting.




AUTO-CLEANUP OPTION

Do you want me to also add an auto-cleanup option (e.g. “keep only the last N
backups” so old ones don’t pile up)?

Perfect 😎 Let’s add automatic cleanup so only the last N backups are kept. I’ll
integrate it into the script:

#!/bin/bash
# Backup script: compress, verify, compare file lists, report size savings, move archive with timestamp, auto-clean old backups
# Usage: ./backup.sh "Folder Name With Spaces" /path/to/backup/location [keep_last_N]

set -euo pipefail

# Input arguments
FOLDER="$1"
BACKUP_DIR="${2:-$HOME/backups}"   # default backup location
KEEP_LAST="${3:-5}"                # default: keep last 5 backups

# Ensure backup directory exists
mkdir -p "$BACKUP_DIR"

# Add timestamp (YYYY-MM-DD)
DATESTAMP=$(date +%F)

# Output archive name (replace spaces with underscores + timestamp)
BASENAME="${FOLDER// /_}"
ARCHIVE="${BASENAME}_${DATESTAMP}.tar.xz"
ARCHIVE_PATH="$BACKUP_DIR/$ARCHIVE"

echo "📦 Creating archive: $ARCHIVE_PATH"
tar -cvJf "$ARCHIVE_PATH" "$FOLDER"

echo "🔍 Testing archive integrity..."
xz -t "$ARCHIVE_PATH"

echo "📋 Comparing file lists..."
diff -u \
  &lt;(find "$FOLDER" -type f | sort) \
  &lt;(tar -tvJf "$ARCHIVE_PATH" | awk '{print $6}' | sort)

if [ $? -eq 0 ]; then
    echo "✅ Backup verified: $ARCHIVE_PATH"
else
    echo "❌ Differences found! Check the lists above."
    exit 1
fi

# --- Size check ---
echo "📊 Checking size savings..."
FOLDER_SIZE=$(du -sh "$FOLDER" | cut -f1)
ARCHIVE_SIZE=$(du -sh "$ARCHIVE_PATH" | cut -f1)

FOLDER_BYTES=$(du -sb "$FOLDER" | cut -f1)
ARCHIVE_BYTES=$(du -sb "$ARCHIVE_PATH" | cut -f1)

SAVED=$((FOLDER_BYTES - ARCHIVE_BYTES))
PERCENT=$((100 * SAVED / FOLDER_BYTES))

echo "📂 Original folder: $FOLDER_SIZE"
echo "🗜️  Archive size:  $ARCHIVE_SIZE"
echo "💾 Space saved:   $((SAVED / 1024 / 1024)) MB (~$PERCENT%)"
echo "📁 Archive stored at: $ARCHIVE_PATH"

# --- Auto-clean old backups ---
echo "🧹 Cleaning up old backups (keeping last $KEEP_LAST)..."
BACKUPS=( $(ls -1t "$BACKUP_DIR"/"${BASENAME}"_*.tar.xz) )
COUNT=${#BACKUPS[@]}

if [ "$COUNT" -gt "$KEEP_LAST" ]; then
    TO_DELETE=( "${BACKUPS[@]:$KEEP_LAST}" )
    for FILE in "${TO_DELETE[@]}"; do
        echo "🗑️  Removing old backup: $FILE"
        rm -f "$FILE"
    done
else
    echo "✅ No old backups to remove."
fi


--------------------------------------------------------------------------------


🔧 HOW TO USE

./backup.sh "Wentworth Castle" /mnt/backup 3

 * Creates a dated archive in /mnt/backup
 * Keeps only the last 3 backups for this folder
 * Automatically deletes older ones
 * Verifies archive integrity, compares file lists, and reports size savings

--------------------------------------------------------------------------------

This gives you a fully automated, versioned, self-cleaning backup system.




SUPPORT MULTI FOLDERS IN A SINGLE RUN

Perfect 😎 — we can extend the script to handle multiple folders in one run.
Each folder will get its own dated archive, integrity check, file list
comparison, size report, and cleanup. Here’s the upgraded script:

#!/bin/bash
# Backup script: compress multiple folders, verify, compare file lists, report size savings, move archive with timestamp, auto-clean old backups
# Usage: ./backup.sh /path/to/backup/location keep_last_N "Folder1" "Folder2" ...

set -euo pipefail

# Input arguments
BACKUP_DIR="$1"             # backup location
KEEP_LAST="$2"              # how many backups to keep per folder
shift 2                     # remaining arguments are folders

FOLDERS=("$@")

# Ensure backup directory exists
mkdir -p "$BACKUP_DIR"

for FOLDER in "${FOLDERS[@]}"; do
    if [ ! -d "$FOLDER" ]; then
        echo "⚠️  Folder not found, skipping: $FOLDER"
        continue
    fi

    BASENAME="$(basename "$FOLDER")"
    BASENAME="${BASENAME// /_}"
    DATESTAMP=$(date +%F)
    ARCHIVE="${BASENAME}_${DATESTAMP}.tar.xz"
    ARCHIVE_PATH="$BACKUP_DIR/$ARCHIVE"

    echo -e "\n📦 Backing up folder: $FOLDER"
    echo "Archive: $ARCHIVE_PATH"

    # Create archive
    tar -cvJf "$ARCHIVE_PATH" "$FOLDER"

    # Test archive integrity
    echo "🔍 Testing archive integrity..."
    xz -t "$ARCHIVE_PATH"

    # Compare file lists
    echo "📋 Comparing file lists..."
    diff -u \
        &lt;(find "$FOLDER" -type f | sort) \
        &lt;(tar -tvJf "$ARCHIVE_PATH" | awk '{print $6}' | sort) || echo "⚠️  Differences found!"

    # Size report
    FOLDER_SIZE=$(du -sh "$FOLDER" | cut -f1)
    ARCHIVE_SIZE=$(du -sh "$ARCHIVE_PATH" | cut -f1)
    FOLDER_BYTES=$(du -sb "$FOLDER" | cut -f1)
    ARCHIVE_BYTES=$(du -sb "$ARCHIVE_PATH" | cut -f1)
    SAVED=$((FOLDER_BYTES - ARCHIVE_BYTES))
    PERCENT=$((100 * SAVED / FOLDER_BYTES))

    echo "📂 Original folder: $FOLDER_SIZE"
    echo "🗜️  Archive size:  $ARCHIVE_SIZE"
    echo "💾 Space saved:   $((SAVED / 1024 / 1024)) MB (~$PERCENT%)"

    # Auto-clean old backups
    echo "🧹 Cleaning up old backups (keeping last $KEEP_LAST)..."
    BACKUPS=( $(ls -1t "$BACKUP_DIR"/"${BASENAME}"_*.tar.xz 2&gt;/dev/null) )
    COUNT=${#BACKUPS[@]}
    if [ "$COUNT" -gt "$KEEP_LAST" ]; then
        TO_DELETE=( "${BACKUPS[@]:$KEEP_LAST}" )
        for FILE in "${TO_DELETE[@]}"; do
            echo "🗑️  Removing old backup: $FILE"
            rm -f "$FILE"
        done
    else
        echo "✅ No old backups to remove."
    fi
done


--------------------------------------------------------------------------------


🔧 HOW TO USE

./backup.sh /mnt/backup 3 "Wentworth Castle" "Family Photos 2022" "Projects"


 * /mnt/backup → where archives go
 * 3 → keep last 3 backups per folder
 * Each folder gets a timestamped archive
 * Each archive is verified, compared, and size-reported
 * Old backups beyond the last 3 are automatically deleted

--------------------------------------------------------------------------------

This is now a full-featured multi-folder, automated, versioned backup system.



Perfect! Here’s a quick one-liner to back up a single folder using the script we
built:

Assuming your script is saved as backup.sh and is executable:

./backup.sh ~/backups 3 "Wentworth Castle"


✅ What this does:
 * Saves the archive in ~/backups
 * Keeps the last 3 backups of this folder
 * Creates a timestamped .tar.xz archive
 * Verifies integrity, compares file lists, and reports size savings
 * Automatically deletes older backups beyond the last 3

--------------------------------------------------------------------------------

For multiple folders at once, just add more folder names after the first:

./backup.sh ~/backups 3 "Wentworth Castle" "Family Photos 2022" "Projects"


This will process each folder individually with the same features.

--------------------------------------------------------------------------------




SHORTCUT ALIAS TO RUN BACKUPS

Perfect 😄 — here's a shortcut alias so you can run backups with just:

backup "Folder Name"

Here’s how to set it up:

--------------------------------------------------------------------------------


1️⃣ OPEN YOUR SHELL CONFIG FILE

 * For bash: ~/.bashrc
 * For zsh: ~/.zshrc

nano ~/.bashrc   # or ~/.zshrc

--------------------------------------------------------------------------------


2️⃣ ADD THE ALIAS FUNCTION

backup() {
    local folder="$1"
    local backup_dir="${2:-$HOME/backups}"  # default backup location
    local keep_last="${3:-3}"                # default: keep last 3 backups

    ~/path/to/backup.sh "$backup_dir" "$keep_last" "$folder"
}

> Replace ~/path/to/backup.sh with the full path to your script.

--------------------------------------------------------------------------------


3️⃣ RELOAD YOUR SHELL

source ~/.bashrc   # or source ~/.zshrc

--------------------------------------------------------------------------------


4️⃣ USE THE SHORTCUT

backup "Wentworth Castle"

 * Saves to ~/backups by default
 * Keeps last 3 backups
 * All verification, size reporting, and cleanup happens automatically

📌 Optional: you can also specify a different backup folder and number of
versions:

backup "Projects" "/mnt/backup" 5

--------------------------------------------------------------------------------

If you want, I can also show a variant that supports multiple folders at once
using the same alias, so you can just do:

backup "Folder1" "Folder2"

Do you want me to do that?

