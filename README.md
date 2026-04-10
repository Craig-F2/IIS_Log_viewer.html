# IIS Log Viewer

A lightweight, self-contained browser-based tool for loading, filtering, grouping, and analyzing IIS web server log files. Everything runs locally in your browser — no installation, no server, no data ever leaves your machine.

---

## Getting Started

1. Download `iis-log-viewer.html`
2. Double-click it to open in any modern browser (Chrome, Edge, or Firefox recommended)
3. Drop one or more IIS log files onto the drop zone, or click to browse

That's it. No dependencies to install, no configuration required.

---

## Requirements

- A modern web browser (Chrome 90+, Edge 90+, Firefox 88+)
- IIS log files in **W3C Extended Log Format** (the default IIS logging format)
  - Files must contain a `#Fields:` header line
  - Standard `.log` or `.txt` extension
- No internet connection required after the file is opened

---

## Log Format

This tool expects the standard W3C Extended Log Format that IIS generates by default. Log files are typically found at:

```
C:\inetpub\logs\LogFiles\W3SVC<site-id>\
```

Each log file should have a header block like:

```
#Software: Microsoft Internet Information Services 10.0
#Version: 1.0
#Date: 2024-01-15 00:00:00
#Fields: date time s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) sc-status sc-substatus sc-win32-status time-taken
```

---

## Features

### Multi-file support
- Load multiple log files at once by dropping them all onto the drop zone, or add them one at a time
- Files are merged and sorted chronologically into a single unified view
- Each loaded file appears as a chip at the top — click **×** to remove a file without losing the others
- A **source file** column and filter let you isolate rows from a specific file at any time

### Filter bar
Filters apply across all three views (Raw logs, Group by, Timeline) in real time.

| Filter | Description |
|---|---|
| Search all fields | Free-text search across every column in a row |
| Status code | Dropdown of all status codes found in the loaded logs |
| Method | Dropdown of all HTTP methods (GET, POST, etc.) |
| Source file | Isolate rows from one specific log file |
| Date from / Date to | Restrict the view to a date range |

Click **Clear** to reset all filters back to defaults.

### Raw logs view
- Full scrollable, paginated table of all log entries
- Every column from the `#Fields:` header is displayed automatically
- Click any column header to sort ascending or descending
- Status codes are color-coded:
  - 🟢 **2xx** — success
  - 🟡 **3xx** — redirect
  - 🟠 **4xx** — client error
  - 🔴 **5xx** — server error
- Long field values are truncated in the cell; hover to see the full value
- Configurable page size: 50 / 100 / 250 / 500 rows per page
- **Export CSV** downloads the currently filtered rows

### Group by view
Aggregate all filtered rows by any field in your log.

- **Field** — pick any column to group on (e.g. `c-ip`, `sc-status`, `cs-uri-stem`, `cs-method`, source file, etc.)
- **Sort by** — count high→low, count low→high, value A–Z, value Z–A, errors high→low, or error % high→low
- **Show top** — limit results to top 25, 50, 100, or show all groups

Each group row shows:
- A proportional bar for quick visual comparison
- Request count and percentage of total filtered rows
- Error count (colored amber/red when elevated)
- Error percentage (turns red above 10%)
- Total bytes transferred (if `sc-bytes` is present in your log)

**Export CSV** saves the grouped summary table.

### Timeline view
A bar chart of request volume over time, split into total requests and 4xx/5xx errors.

- **Hour** — useful for a single day's log
- **Day** — useful for logs spanning days or weeks  
- **Week** — useful for logs spanning months

The chart updates live as filters change, so you can visualize traffic patterns for a specific IP, URL, or status code.

### Stats bar
Four summary cards at the top update live as you filter:

- **Total requests** — all rows across all loaded files
- **Filtered rows** — rows matching current filters
- **Unique IPs** — distinct client IPs in filtered results
- **Errors (4xx/5xx)** — error count in filtered results

---

## Tips

- **Finding abusive IPs:** Use Group by `c-ip`, sort by Count or Error % to surface the top talkers or most error-prone clients
- **Spotting broken pages:** Group by `cs-uri-stem`, sort by Error % to find URLs generating the most 404s or 500s
- **Analyzing a specific IP:** Set the search box to an IP address, then switch to Group by `cs-uri-stem` to see exactly which URLs that IP requested
- **Combining filters with group by:** All filters apply before grouping, so you can e.g. filter to `sc-status = 500` then group by `cs-uri-stem` to find which pages are generating server errors
- **Large date ranges:** Load multiple daily log files at once — they merge automatically into one chronological view

---

## Privacy & Security

- All processing happens entirely in your browser
- Log files are never uploaded or transmitted anywhere
- No external services, tracking, or analytics
- Works fully offline once the `.html` file is open

---

## Limitations

- Supports W3C Extended Log Format only (not NCSA Common or IIS native format)
- Very large files (100MB+) may be slow to parse depending on your browser and machine
- Binary or non-UTF-8 encoded log files are not supported

---

## License

Free to use and modify. No warranty expressed or implied.
