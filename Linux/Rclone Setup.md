# Setting Up rclone with Google Drive on Debian/GNOME

## Prerequisites
- ✅ rclone installed
- ✅ Google API project ("rclone-drive") created
- You'll need the **Client ID** and **Client Secret** from your Google Cloud Console

---

[my rclone api](https://console.cloud.google.com/apis/dashboard?project=rclone-drive-489709)

## Step 1: Get Your API Credentials

If you haven't already created OAuth credentials within your project:

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Select your **rclone-drive** project
3. Navigate to **APIs & Services → Credentials**
4. Click **Create Credentials → OAuth client ID**
5. Application type: **Desktop app**
6. Note down the **Client ID** and **Client Secret**

Also ensure the **Google Drive API** is enabled:
- **APIs & Services → Library → Google Drive API → Enable**

---

## Step 2: Configure rclone

Run the interactive configuration:

```bash
rclone config
```

You'll see a menu:

```
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q>
```

### Walk through the prompts:

```
n/s/q> n
```

```
name> gdrive
```

You'll see a long list of storage types. Find Google Drive:

```
Storage> drive
```

When prompted for **Client ID**:
```
client_id> YOUR_CLIENT_ID_HERE
```

When prompted for **Client Secret**:
```
client_secret> YOUR_CLIENT_SECRET_HERE
```

### Scope selection:
```
scope> 1
```

| Option | Access Level |
|--------|-------------|
| **1** | Full access to all files (recommended) |
| 2 | Read-only access |
| 3 | Access to files created by rclone only |
| 4 | Read-only access to file metadata |
| 5 | Full access to files in Application Data folder only |

### Remaining prompts:

```
service_account_file> 
```
*(Leave blank, just press Enter)*

```
Edit advanced config?
y/n> n
```

```
Use auto config?
y/n> y
```

---

## Step 3: Browser Authentication

After selecting auto config, rclone will:

1. Start a local web server
2. **Automatically open your default browser** (on GNOME/Debian, this should work out of the box)
3. Display a Google sign-in/consent page

```
If your browser doesn't open automatically go to the following link:
http://127.0.0.1:53682/auth?state=XXXXXXXX
Log in and authorize rclone for access
Waiting for code...
```

- **Sign in** with your Google account
- **Grant permissions** to rclone
- You'll see a success message: *"Success! All done. Please go back to rclone."*

> ⚠️ If you get an "unverified app" warning, click **Advanced → Go to rclone-drive (unsafe)** — this is expected for personal API projects.

---

## Step 4: Finish Configuration

```
Configure this as a Shared Drive (Team Drive)?
y/n> n
```

You'll see a summary:

```
[gdrive]
type = drive
client_id = YOUR_CLIENT_ID
client_secret = YOUR_CLIENT_SECRET
scope = drive
token = {"access_token":"...","token_type":"Bearer",...}
```

```
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y
```

Then quit:
```
q) Quit config
q
```

---

## Step 5: Verify It Works

### List files in your Google Drive root:
```bash
rclone ls gdrive:
```

### List only directories:
```bash
rclone lsd gdrive:
```

### List a specific folder:
```bash
rclone ls gdrive:Documents
```

---

## Common Usage Examples

### Copy a file to Google Drive:
```bash
rclone copy /home/username/file.txt gdrive:Backups/
```

### Sync a local folder to Drive:
```bash
rclone sync /home/username/Documents gdrive:Documents
```

> ⚠️ `sync` makes the destination **identical** to the source (deletes extra files on remote). Use `copy` if you don't want deletions.

### Mount Google Drive as a filesystem:
```bash
mkdir -p ~/GoogleDrive
rclone mount gdrive: ~/GoogleDrive --vfs-cache-mode full &
```

### Interactive file browser (ncurses-style):
```bash
rclone ncdu gdrive:
```

---

## Optional: Verify/Edit Config Later

Your configuration is stored at:
```
~/.config/rclone/rclone.conf
```

To reconfigure:
```bash
rclone config
```

To check your connection anytime:
```bash
rclone about gdrive:
```

This will show your Drive storage usage/quota.
