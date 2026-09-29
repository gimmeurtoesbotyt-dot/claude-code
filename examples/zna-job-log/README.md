# ZnA Job Log

A CRM page for a detailing business. It runs as a claude.ai Artifact on top of an existing
Google Form → Google Sheet job log. The sheet stays the source of truth. The page reads it live
through the viewer's own Google Sheets connector.

## What it does

- **Overview**: revenue, job count, average ticket and $/hour for this month compared with last month;
  repeat clients past their usual gap ("Due for a visit"); revenue by month; revenue and $/hour per
  service; best clients; one-time clients worth winning back.
- **Clients**: jobs grouped into clients by name, and by phone number when the name was typed
  differently. Each client shows lifetime spend, visits, usual gap between visits, vehicles and full
  history. When one person was logged under different names, **Same person as another client?** in
  their profile merges them. Merges are saved in the artifact's own store (the `merges` collection),
  never in the sheet, and each one can be undone.
- **Jobs**: every job, searchable, filterable by service and month, with totals.
- **Log a job**: appends a row to the `App Log` tab (same columns as the form, plus Notes).
  Rows are not written into `Form Responses 1`, so they can't collide with Google Form submissions.
- **Ask Claude**: questions about the job log, answered from the data on the page.

## Sheet layout it expects

`Form Responses 1` and `App Log`, with a header row of:
Timestamp, Client Info, Date of Service, Vehicle Info, Service Provided, Pricing, Payment Method,
Client Phone Number, Time Spent, Hourly Rate, and an optional 11th column (Notes).
Columns are matched by header name, so the order can change.

## Setup

1. Replace `__SHEET_ID__` in `index.html` with the ID from the sheet's URL
   (`docs.google.com/spreadsheets/d/<ID>/...`). Don't commit the real ID to a public repo.
2. Publish `index.html` as an Artifact with these capabilities:
   `{"mcp": {"servers": [{"server": "Google Sheets", "tools": ["get_values", "append_values"]}]}, "sample": {}, "db": {}}`
3. The viewer needs the Google Sheets connector in claude.ai Settings → Connectors and edit access
   to the sheet.
