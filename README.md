## Alex Resource Hub

Mobile-first, password-protected dashboard that gives Alex’s family one tap access to Google Drive folders, medical history, benefits paperwork, and crisis resources. The UI uses large tiles (2×2 on phones) with strong contrast modes and adjustable text sizing to keep things senior-friendly.

### Files
- `auth.html` – lightweight login screen that gates the hub behind the shared password (`alex123` by default). Includes contrast + font controls so caregivers can tweak readability before logging in.
- `index.html` – the actual dashboard: catch-up timeline, quick-action tiles, live search, and folder breakdown. Everything is driven by JS data so you only edit links in one place.
- `standalone-dashboard.html` – older Tailwind prototype kept for reference.

### How to run
Open `auth.html` in any browser (double-click or serve via `python -m http.server`). Once authenticated, the app stores `alexHubAuthed=true` in localStorage and routes to `index.html`.

### Changing the password
Edit the `PASSWORD` constant in `auth.html`. The same value is compared client-side, so keep the file private or host it behind real auth if you need stronger security.

### Wiring up Google Drive
Inside `index.html`, look for the `HUB_LINKS` object. Replace each `REPLACE_…` placeholder with the actual Drive folder/document URL. Every tile, folder card, and search result references those keys, so you only need to paste the URL once.

Tip: if a link still contains `REPLACE_`, the UI will alert the user instead of navigating—handy while you’re still setting up the Drive tree.

### Editing the catch-up feed
Update the `RECENT_UPDATES` array in `index.html`:
```js
const RECENT_UPDATES = [
  { summary: "Housing voucher submitted", timeAgo: "Nov 20", author: "Jamie" },
  // ...
];
```
The card at the top of the dashboard renders this list in order. Hook it up to a shared “update log” Google Doc by replacing `HUB_LINKS.catchupLog`.

### Maintaining the resource index
The `RESOURCE_INDEX` array mirrors the folder structure you outlined (Key Info, Medical, Behavioral Health, etc.). Add/remove entries as your Drive changes—search will instantly reflect the updates without touching the layout.

### Accessibility controls
- **High contrast**: toggled from both `auth.html` and `index.html`. Preference is saved in `localStorage` under `alexHubContrast`.
- **Text size**: slider on the login screen + select dropdown on the dashboard save their value to `alexHubBaseFont`.
- **Keyboard shortcuts**: `Ctrl + K` (or `Cmd + K`) focuses the search bar.

### Deploying
Any static host works (Netlify, GitHub Pages, Cloudflare Pages). Just upload the HTML files as-is. If you prefer a single-page bundle, keep `auth.html` and `index.html` at the same directory depth so the redirects keep working.
