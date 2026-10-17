# Raven Lake Hunt Camp — GitHub Pages PWA

## What's included

- `index.html`, `style.css`, `app.js` — the app
- `manifest.webmanifest`, `sw.js`, `assets/icon-*.png` — installable PWA files
- `assets/members/` — nine photos from your uploaded workbook
- `assets/camp/` — five photos from your workbook
- `config.js` — the only website file you must edit to connect Google Sheets
- `apps-script/Code.gs` — copy this into your spreadsheet's Google Apps Script editor

## 1. Prepare Google Sheets

Upload the **original** Raven Lake workbook (without embedded photos) to Google Drive and open it as a Google Sheet. Keep these exact tab names: `Setup`, `Members`, `Camp Photos`, `Attendance`, `Meals`, `Costco List`, `Camp Overview`.

In `Members`, enter **one member per consecutive row starting at row 5**:

| Member ID | First Name | Display Name (optional) | Photo URL | Active? |
|---|---|---|---|---|
| M01 | MA | | | Yes |
| M02 | JOFO | | | Yes |
| M03 | FORDO | | | Yes |
| M04 | BEAR | | | Yes |
| M05 | CASS | | | Yes |
| M06 | WHIRLY | | | Yes |
| M07 | MAJOR | | | Yes |
| M08 | POPCORN | | | Yes |
| M09 | PETEY | | | Yes |

Clear or replace any sample member records. **JOFO must use M02, not M01.** The app pairs member pictures with M01–M09. Sheet photo URLs are not used in this version. Keep the existing dates, attendance, meals and Costco headers. Do not rename these tabs or alter the header columns.

Dates: November 1 through November 8, 2026 **inclusive: eight calendar dates**. (Sunday to the following Sunday is eight calendar dates, seven nights.) The app intentionally follows the workbook's 8-day schedule.

## 2. Connect the Google Sheet

1. In the Google Sheet choose **Extensions → Apps Script**.
2. Delete the example `myFunction()` code. Paste the entire contents of `apps-script/Code.gs` into `Code.gs`.
3. Click **Save** and name the project `Raven Lake Connector`.
4. Select **Deploy → New deployment**; type **Web app**.
5. Set **Execute as: Me** and **Who has access: Anyone** (not "Anyone with Google account").
6. Click **Deploy**, authorize permissions for your own Google account, then copy the Web app URL ending in `/exec`.
7. In `config.js` replace `''` with that URL in single quotes. Example: `window.RAVEN_LAKE_API_URL = 'https://script.google.com/macros/s/XXXXX/exec';`.
8. When you edit Apps Script later, select **Deploy → Manage deployments → Edit → New version → Deploy**. The existing `/exec` URL typically stays the same.

The script reads/writes your Sheet under **your account**. Other members need no Google account. Member-photo selection is **not authentication**; anyone who knows the app or endpoint link could impersonate a member and modify records. Do not put private information in this sheet; don't share the Apps Script URL publicly. Apps Script web apps set to "Anyone" are public endpoints. This simple setup is suitable only for a small group that accepts that risk.

## 3. Upload to GitHub

1. Create a GitHub repository, e.g. `raven-lake` (choose a private repo only if you understand that its GitHub Pages site may still be publicly accessible).
2. Upload **the CONTENTS** of this folder to the root of the repository, preserving folders (`assets`, `apps-script`). It is okay to upload the ZIP contents in several batches; commit all files.
3. Open **Settings → Pages → Build and deployment**. Choose **Deploy from a branch**, `main`, `/ (root)`, **Save**.
4. Open `https://YOUR-USERNAME.github.io/raven-lake/` (your actual repo name determines the address).
5. On iPhone: Safari → Share → Add to Home Screen. On Android: Chrome → three-dot menu → Add to Home screen / Install app.

Your **members do not need GitHub logins**. The website and profile photos are public to anyone with its URL. For strict privacy, use an authenticated hosting setup instead.

## 4. Verify before inviting camp members

- Tap a member picture and select a camp day. Check the `Attendance` Sheet tab updated to `Yes` in the correct member row/day column.
- In **Cooking**, assign a cook and menu. Check `Meals` columns D and F.
- Add a Costco item, then mark it purchased. Check the `Costco List` tab.
- Open the app on a second device: changes should appear after a refresh or within roughly 20 seconds while the app is active.
- If something does not save, verify the correct `/exec` URL, the access setting, and the member IDs.

## Important limitations

- No true sign-in or access control; member pictures are for convenience, not security.
- The Google Sheet is not exposed to visitors, but the Apps Script endpoint is public and can be called by anyone with the URL. The connector uses JSONP, including state-changing requests, to work from GitHub Pages without CORS configuration; this is **not suitable for sensitive data**.
- App reads and updates require an internet connection. Previously cached screens may appear offline, but **offline changes are not saved**.
- The browser polls periodically; data synchronization is near-live rather than push-based.
- If you change photos, replace the corresponding files under `assets/` (and bump the cache version in `sw.js` if old images stick).
- The original template has eight dates. Changing to a future year requires updating the hardcoded dates in both `app.js` and `apps-script/Code.gs`, as well as the spreadsheet. The current version is specifically prepared for **2026**.
- Photos and backgrounds are published as part of the website assets, not inside Sheets.

## App features

Photo profile selection; Sunday–Sunday attendance; automatic breakfast/supper headcounts; daily attendance calendar; open cooking slots and menu editing; editable shared Costco list. Changes save to Google Sheets.
