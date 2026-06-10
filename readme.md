# Leave Analytics Dashboard — Setup Guide

## What you're building

```
[All Center Sheets]  ──►  [Google Apps Script]  ──►  [dashboard.html]
  Data + Manual Entry       (reads all sheets,         (opens in browser,
  sheets per center          returns JSON API)           shows live charts)
```

---

## Step 1 — Open your Master Google Sheet (Center Index)

This is the sheet that contains the `Center Index` tab with all your
center names, Spreadsheet IDs, and region names.

---

## Step 2 — Open Apps Script editor

1. In your Master Sheet, click **Extensions → Apps Script**
2. Delete all existing code in the editor
3. Copy the entire contents of `Code.gs` and paste it in
4. Click **Save** (the floppy disk icon or Ctrl+S)
5. Name your project: `Leave Dashboard API`

---

## Step 3 — Deploy as Web App

1. Click **Deploy → New deployment**
2. Click the gear icon ⚙ next to "Select type" and choose **Web app**
3. Set the following:
   - **Description**: Leave Dashboard API
   - **Execute as**: Me _(your Google account)_
   - **Who has access**: Anyone  
     _(or "Anyone within [your org]" if you want org-only access)_
4. Click **Deploy**
5. **Copy the Web App URL** — it looks like:
   ```
   https://script.google.com/macros/s/AKfycb.../exec
   ```
   Save this URL — you'll paste it into the dashboard.

> **First time only:** Google will ask you to authorise the script.
> Click "Review permissions" → choose your account → click "Allow".

---

## Step 4 — Grant access to all center sheets

The script runs as _you_, so it can access any sheet you have access to.
If any center sheet is owned by someone else:

- Ask the owner to share it with your Google account (Viewer is enough)
- Or set it to "Anyone with the link can view"

---

## Step 5 — Open the dashboard

1. Open `dashboard.html` in any browser  
   _(double-click the file, or drag it into Chrome/Edge/Firefox)_
2. At the top yellow bar, paste your **Web App URL**
3. Click **Connect**

The dashboard will fetch all data and show live charts instantly.

---

## Step 6 — Keep data fresh (optional automation)

### Option A — Manual refresh

Click the **↻ Refresh** button in the top-right of the dashboard anytime.

### Option B — Auto-refresh (add this to Code.gs)

Add a time-driven trigger in Apps Script:

1. In Apps Script, click the **clock icon** (Triggers) in the left sidebar
2. Click **+ Add Trigger**
3. Choose:
   - Function: `updateSyncTimestamps`
   - Event source: Time-driven
   - Type: Every hour (or every 30 minutes)
4. Click **Save**

The dashboard always fetches fresh data on load; the trigger just
keeps the "Last Synced" timestamps in your Center Index up to date.

---

## What the API returns

The script supports three URL parameters:

| URL               | Returns                                |
| ----------------- | -------------------------------------- |
| `?action=all`     | Every leave record from all centers    |
| `?action=summary` | Pre-aggregated totals (faster)         |
| `?action=centers` | Just the center list from Center Index |

**Filter by region or center:**

```
?action=all&region=UP
?action=all&center=Varanasi - BHU Vidyapeeth
?action=summary&region=Delhi + HR
```

---

## Adding new centers

When a new center sheet is added to your `Center Index` tab:

- No code changes needed
- The script automatically picks it up on the next dashboard load
- Just make sure you have access to the new sheet

---

## Troubleshooting

| Problem                     | Fix                                                                                           |
| --------------------------- | --------------------------------------------------------------------------------------------- |
| "Script function not found" | Re-save and re-deploy Code.gs                                                                 |
| "Access denied" error       | Make sure the center sheet is shared with your account                                        |
| "Sheet not found"           | Check that sheet names are exactly `Data` and `Manual Entry (To be done when i`               |
| Dashboard shows no data     | Open the Web App URL directly in browser — check for error messages                           |
| Old data showing            | Re-deploy the script after any code change (Deploy → Manage deployments → Edit → New version) |

---

## Column name mapping (Code.gs reads these automatically)

The script handles slight variations in column names between sheets:

- `Combined  Center` and `Combined Center` (double space vs single)
- `Total Leaves Taken` and `Actual Days` (both used as fallback)

If your sheets use different column headers, update the `REQUIRED_COLS`
array in `Code.gs` to match.

---

## Files in this package

```
leave-dashboard/
├── Code.gs          → Paste into Google Apps Script
├── dashboard.html   → Open in browser (no server needed)
└── SETUP.md         → This guide
```
