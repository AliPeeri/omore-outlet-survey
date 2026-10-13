# Omore Outlet Survey — free offline-first starter

## 1. Google Sheets backend
1. Create a Google Sheet called **Omore Outlet Surveys**.
2. From the Sheet choose Extensions → Apps Script. Replace `Code.gs` with the supplied `Code.gs`.
3. Create a Google Drive folder named **Omore Shop Photos**, copy its folder ID from its URL.
4. In Apps Script, Project Settings → Script properties → Add `PHOTO_FOLDER_ID` with that ID.
5. Run `setup` once in Apps Script and approve requested permissions.
6. Deploy → New deployment → Web app → Execute as **Me** → Who has access **Anyone**. Copy the `/exec` URL. **Security note:** this makes an internet-accessible upload endpoint. It has no user authentication; use only for a controlled pilot, do not distribute URL publicly. For company use, add access controls before rollout.

## 2. Host the Android PWA free
1. Create a public GitHub repository and upload `index.html`, `app.js`, `sw.js`, `manifest.webmanifest` (NOT `Code.gs`).
2. Repository Settings → Pages → Deploy from branch → main/root. Open the HTTPS Pages URL on Android Chrome.
3. Chrome menu → Add to Home screen / Install app. Open app; paste the `/exec` URL in Configuration and Save.
4. Test one survey online, then airplane mode, collect another, turn online and tap **Sync pending surveys**.
5. Export Excel from Google Sheets: File → Download → Microsoft Excel (.xlsx).

## Operational notes
- Android Chrome location permission must be granted. Location capture works offline only when the phone can obtain a position; accuracy can vary. Default accuracy gate: <=100 m (change in `app.js` if needed).
- Records/photos are stored in browser IndexedDB until server confirmation. Do not clear Chrome site data or uninstall the app with pending surveys. Sync at least daily; local data is not a permanent backup.
- The app retries on network return, but a manual **Sync pending surveys** button is the reliable fallback. Each record has an idempotent unique ID to prevent duplicate rows after retries.
- **Photo links are private Google Drive links**; only users with permission to the folder can open them. Excel export contains photo links, not embedded images.
- Google Apps Script, Drive and Sheets have free quotas and limits. Test with real Android devices and real field volume before scaling to 1,500 outlets.
- GitHub Pages is public hosting. Do not place secrets, personal employee information, or the backend script in the repository. Backend URL is saved on each device, and is not a secret/authentication control.
- The form collects personal location only at submission; obtain appropriate employee permission and follow company data/security policy.
- If a deployment fails, the survey remains queued. If the server saved it but the phone missed the acknowledgement, retry will not duplicate the row.
- This is a pilot starter, not a production-secured enterprise application. In particular, backend access control and route-based permissions should be added before large-scale rollout.
