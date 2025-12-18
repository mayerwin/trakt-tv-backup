# Trakt.tv Backup Script (PowerShell)

A standalone, single-file PowerShell script to backup your entire [Trakt.tv](https://trakt.tv) user profile. 

It uses the **OAuth Device Flow** for authentication, meaning you can run it purely from the command line without needing a web server or complex callback URLs.

## 🚀 Features

* **Zero Dependencies:** Runs on standard PowerShell (Windows/Linux/macOS).
* **Complete Backup:** Downloads data that standard CSV exports miss:
    * Watchlist, Ratings, Collection, Watched History
    * **Custom Lists** (including the items inside them)
    * Comments, Likes, Social Graph (Friends/Following)
    * Account Settings & Playback Progress
* **Self-Healing:** Automatically handles token refreshing. Run it once to authorize, and it will keep working forever (as long as it runs at least once every 3 months).
* **Smart Pagination:** Handles large libraries by automatically looping through pages.
* **Portable:** Creates a neat `.zip` file with all your data.

## 🛠️ Setup

### 1. Create a Trakt API App
To use this script, you (or the user) need a Client ID and Secret from Trakt.
1.  Go to [Trakt API Applications](https://trakt.tv/oauth/applications).
2.  Click **New Application**.
3.  **Name:** `My Backup Script` (or anything you like).
4.  **Redirect URI:** `urn:ietf:wg:oauth:2.0:oob` (Important!).
5.  Save the app.
6.  Copy the **Client ID** and **Client Secret**.

### 2. Installation
Clone this repository or download the script.
```powershell
git clone [https://github.com/YOUR_USERNAME/trakt-powershell-backup.git](https://github.com/YOUR_USERNAME/trakt-powershell-backup.git)
cd trakt-powershell-backup
```

## 💻 Usage

### First Run (Authentication)
Run the script passing your credentials. You will be prompted to visit a URL and enter a code to authorize the script.

```powershell
.\trakt_backup.ps1 -ClientId "YOUR_CLIENT_ID" -ClientSecret "YOUR_CLIENT_SECRET"
```

Once authorized, the script will:
1.  Save your tokens securely to `trakt_secrets.json`.
2.  Perform the first full backup.
3.  Generate a `.zip` file in the same folder.

### Subsequent Runs (Automation)
For future runs, you do not need to provide arguments. The script will load the saved tokens from `trakt_secrets.json`.

```powershell
.\trakt_backup.ps1
```

If the access token is expired, the script will automatically use the refresh token to get a new one and update the `trakt_secrets.json` file.

## 🤖 Automation (Windows Task Scheduler)
You can set this to run weekly or monthly.

1.  Open **Task Scheduler**.
2.  Create a Basic Task.
3.  **Action:** Start a Program.
4.  **Program/script:** `powershell.exe`
5.  **Add arguments:** `-ExecutionPolicy Bypass -File "C:\Path\To\trakt_backup.ps1"`
6.  **Start in:** `C:\Path\To\` (Important: must be the folder containing the secrets file).

## 🔒 Security Note
* **trakt_secrets.json**: This file contains your access credentials. **NEVER** share this file or commit it to GitHub.
* **Backups**: The generated `.zip` files contain your personal viewing history.

## 📄 License
MIT License
