# Tqind — New Minimal Site

What’s inside
- `index.html` — hero + **mailing list signup** + link to Lyric Square demo + News link
- `lyric.html` — branded **interactive 24h line‑graph** for Lyric Square + **Is this about right?** yes/no + comments
- `news.html` — placeholder
- `styles.css` — minimalist brand
- `script.js` — posts signups and feedback to Google Apps Script → Google Sheet

Setup
1) Attach this Apps Script to your Google Sheet (Extensions → Apps Script) and deploy as Web App:
```javascript

/**
 * Google Apps Script for Tqind forms
 * Attach to your target Google Sheet (Extensions → Apps Script).
 * Deploy as Web app (Execute as: Me, Who has access: Anyone).
 * Tabs used: 'emails', 'feedback'
 */
const SHEET_NAMES = { signup:"emails", feedback:"feedback" };

function doPost(e) {
  try {
    const body = JSON.parse(e.postData.contents || "{}");
    const ss = SpreadsheetApp.getActiveSpreadsheet();
    if (body.type === "signup") {
      const sh = ss.getSheetByName(SHEET_NAMES.signup) || ss.insertSheet(SHEET_NAMES.signup);
      sh.appendRow([new Date(), body.email || "", body.borough || ""]);
    } else if (body.type === "feedback") {
      const sh = ss.getSheetByName(SHEET_NAMES.feedback) || ss.insertSheet(SHEET_NAMES.feedback);
      sh.appendRow([new Date(), body.location || "", body.aboutRight || "", body.comments || ""]);
    } else {
      const sh = ss.getSheetByName("misc") || ss.insertSheet("misc");
      sh.appendRow([new Date(), JSON.stringify(body)]);
    }
    return ContentService.createTextOutput(JSON.stringify({ ok: true })).setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({ ok: false, error: String(err) })).setMimeType(ContentService.MimeType.JSON);
  }
}

```
2) Copy the Web App URL and replace `WEB_APP_URL` in `script.js`.
3) Push these files to a GitHub repo and enable **GitHub Pages**.

Brand
- Replace `logo.png` with your logo (same filename).
- Tweak colors in `:root` inside `styles.css`.

© 2025 Tqind
