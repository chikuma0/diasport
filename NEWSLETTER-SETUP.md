# Newsletter Signup — Google Sheets Backend

Since Formspree free tier is full, we use a Google Apps Script as a free webhook.

## Setup (one-time, ~3 minutes)

1. Go to https://sheets.google.com → Create new spreadsheet
2. Name it "Diasport Signups"
3. In Row 1, add headers: `Email` | `Timestamp`
4. Go to **Extensions → Apps Script**
5. Replace the code with:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  sheet.appendRow([data.email, data.ts || new Date().toISOString()]);
  return ContentService.createTextOutput(JSON.stringify({status: 'ok'}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

6. Click **Deploy → New deployment**
7. Type: **Web app**
8. Execute as: **Me**
9. Who has access: **Anyone**
10. Click **Deploy** → Copy the URL

Then tell Kuro the URL and I'll add it to the site.

## How it works
- Form submits → POST to Apps Script URL
- Apps Script appends row to the spreadsheet
- Also saves to localStorage as backup
- Free, unlimited, no account limits
