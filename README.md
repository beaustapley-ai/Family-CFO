# Family CFO

A household ledger. The app runs on GitHub Pages, and the data stays in your own Google Sheet.

```
GitHub Pages (index.html)  ──fetch──▶  Apps Script /exec  ──▶  Google Sheet
   the UI, no data                     JSON API (Code.gs)        the database
```

## What's in this repo

| Path | What it is |
|---|---|
| `index.html` | The whole app: one file with its CSS and JS inline |
| `manifest.webmanifest`, `icons/` | Lets the app install as a real home-screen app (no Safari bar) |
| `apps-script/Code.gs` | The backend. **Pasting it here does nothing.** It runs inside the Sheet; the copy here is only for the record |

No secrets live in this repo. The backend URL is entered on each device and stored in that browser, so the repo can be public.

## 1 · Update the backend (about 5 minutes)

1. Open the Family CFO Sheet ▸ **Extensions ▸ Apps Script**.
2. Replace everything in `Code.gs` with `apps-script/Code.gs`. You can delete the old `Index.html` file in the script; nothing serves it any more.
3. **Deploy ▸ Manage deployments ▸ ✏️ edit** the existing deployment:
   - Version: **New version**
   - Execute as: **Me**
   - Who has access: **Anyone**
   - Click **Deploy** and approve the permissions prompt. The script now only asks for Sheets access; the Drive scope is gone.
4. Copy the **Web app URL** (it ends in `/exec`). Editing the existing deployment keeps the URL you already have.

To check it works, open the `/exec` URL in a browser. You should see `{"ok":true,"app":"family-cfo",…}`.

> On the first run the script adds a `downloadUrl` column to the **Accounts** tab by itself. Existing data is untouched.

## 2 · Publish the app

1. Create a new GitHub repo, e.g. `family-cfo`, and upload everything in this folder.
2. Go to **Settings ▸ Pages ▸ Build and deployment**, choose **Deploy from a branch**, then `main` / `/ (root)`.
3. After a minute it's live at `https://<you>.github.io/family-cfo/`.

## 3 · Connect each device

- Open the Pages URL, paste the `/exec` URL, and enter your name (it's stamped on the imports you upload).
- **Partner:** go to Setup ▸ This device ▸ **Copy setup link** and text it to them. Opening the link sets up their device in one tap.
- **iPhone:** open the link in Safari, then go to **Share ▸ Add to Home Screen**. If the home-screen icon asks to connect, paste the link once more; iOS keeps home-screen apps' storage separate from Safari's.
- **Waypoint:** put the new Pages URL in the Sheet's `App!A1` so the hub launches the new version.

## 4 · Add the bank download links

Setup ▸ Accounts has a **CSV download page** field for each account. Paste the exact page where you click Download. A **Download** button then appears on Overview (in the Accounts card) and on Import, next to the account you picked.

## Things to know

- **Link-only access.** Anyone who has the `/exec` URL can read the ledger. Only share the setup link with people you'd share the Sheet with. If it ever leaks, use **Deploy ▸ New deployment** to get a new URL and archive the old one.
- **Instant open.** Each device keeps its last copy of the ledger in browser storage. The app shows that copy right away and then syncs, which takes about 1–2 seconds. Setup ▸ **Forget this device** clears it.
- **Updating the app** means replacing `index.html` in the repo. **Updating the backend** means pasting the new Code.gs, then going to Manage deployments ▸ edit ▸ New version (never "New deployment", which changes the URL).
- Export now downloads straight to the device instead of writing a file to Drive.
