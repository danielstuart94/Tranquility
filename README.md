
# Tqind – Minimal Static Site

Minimal, brand-aligned site for GitHub Pages.

## Pages
- `index.html` – landing
- `signup.html` – email signup
- `news.html` – coming soon
- `data.html` – slider + yes/no capture

## Setup
1. In your Google Sheet (the one you linked), open **Extensions → Apps Script** and paste the code below.
2. **Deploy** as a Web App (Execute as *Me*, access *Anyone*). Copy the URL.
3. In `script.js`, replace `DEPLOYMENT_ID` with the Apps Script deployment ID or full URL.
4. Upload all files to a GitHub repo and enable **GitHub Pages**.

## Apps Script
```javascript

const SHEET_NAMES = { signup: "emails", observation: "responses" };

function doPost(e) {
  try {
    const body = JSON.parse(e.postData.contents);
    const ss = SpreadsheetApp.getActiveSpreadsheet();
    const sheetName = SHEET_NAMES[body.type] || "responses";
    const sheet = ss.getSheetByName(sheetName) || ss.insertSheet(sheetName);
    if (body.type === "signup") {
      sheet.appendRow([new Date(), body.email || "", body.borough || ""]);
    } else if (body.type === "observation") {
      sheet.appendRow([new Date(), body.location || "", body.rating || "", body.is_tranquil || ""]);
    } else {
      sheet.appendRow([new Date(), JSON.stringify(body)]);
    }
    return ContentService.createTextOutput(JSON.stringify({ ok: true })).setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({ ok: false, error: err.toString() })).setMimeType(ContentService.MimeType.JSON);
  }
}

```

## Branding
Edit colors in `styles.css` via CSS variables `--brand` and `--accent`. Replace `logo.png` with your logo.

## Repo privacy
- **Public**: everyone can see and copy your code. Choose a license or leave “All rights reserved.”
- **Private**: only collaborators can see it. GitHub Pages can publish from a private repo if you flip Pages on.
