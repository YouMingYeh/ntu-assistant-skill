# myNTU Portal Reference

myNTU (`my.ntu.edu.tw`) is NTU's main academic portal built on ASP.NET WebForms.

## Key URLs

| Page | URL |
|------|-----|
| Timetable (課表) | `https://my.ntu.edu.tw/academia/currTimetable.aspx` |
| Enrollment (選課) | `https://my.ntu.edu.tw/academia/currCourse.aspx` |
| Grade Query (成績) | `https://my.ntu.edu.tw/academia/gradeQry.aspx` |
| Portal Home | `https://my.ntu.edu.tw/` |

## Authentication

- myNTU uses NTU SSO but may have a **separate session** from COOL
- After navigating to myNTU, always `take_snapshot` to verify login state
- If not logged in: user will see NTU SSO login page → prompt them to log in manually
- SSO may redirect through `web2.cc.ntu.edu.tw` or `adfs.ntu.edu.tw`

## Timetable Extraction

The timetable page renders as an HTML `<table>` with time slots as rows and weekdays as columns.

### ASP.NET WebForms Quirks
- Pages use ViewState (large hidden form fields) — ignore these
- Some content loads via postback — may need to wait for full render
- iframes may be present — check if content is in a frame

### Extraction via evaluate_script

```javascript
// Extract timetable from the HTML table
const table = document.querySelector('table.tableCurrTimetable') ||
              document.querySelector('#ContentPlaceHolder1_timetable') ||
              document.querySelector('table[id*="Timetable"]');

if (!table) {
  JSON.stringify({error: 'Timetable table not found'});
} else {
  const rows = Array.from(table.querySelectorAll('tr'));
  const schedule = [];
  rows.forEach((row, i) => {
    const cells = Array.from(row.querySelectorAll('td, th'));
    schedule.push(cells.map(c => c.innerText.trim()));
  });
  JSON.stringify(schedule);
}
```

### Alternative: Full page text extraction
If the table selector doesn't work, use `take_snapshot` and parse the accessibility tree text for course names, times, and locations.

### Schedule Mapping

NTU time slots:
| Slot | Time |
|------|------|
| 1 | 08:10-09:00 |
| 2 | 09:10-10:00 |
| 3 | 10:20-11:10 |
| 4 | 11:20-12:10 |
| 5 (午) | 12:20-13:10 |
| 6 | 13:20-14:10 |
| 7 | 14:20-15:10 |
| 8 | 15:30-16:20 |
| 9 | 16:30-17:20 |
| 10 (傍) | 17:30-18:20 |
| A | 18:25-19:15 |
| B | 19:20-20:10 |
| C | 20:15-21:05 |
| D | 21:10-22:00 |

### Output: schedule.md format
```markdown
# 114-2 Weekly Schedule (課表)

| Time | Mon | Tue | Wed | Thu | Fri |
|------|-----|-----|-----|-----|-----|
| 1 (08:10) | | | | | |
| 2 (09:10) | | | | | 巨量資料系統 @管二104 |
| ... | | | | | |
```

## Grade Query

Navigate to `gradeQry.aspx`, then:
```javascript
// Extract grade table
const table = document.querySelector('table[id*="grade"]') ||
              document.querySelector('.table-grade');
if (table) {
  const rows = Array.from(table.querySelectorAll('tr'));
  const grades = rows.map(r => {
    const cells = Array.from(r.querySelectorAll('td, th'));
    return cells.map(c => c.innerText.trim());
  });
  JSON.stringify(grades);
}
```

## Common Issues

- **Slow page load**: ASP.NET WebForms pages can take 3-5 seconds. Use `wait_for` with appropriate text/selector.
- **ViewState errors**: If page shows ViewState validation error, navigate fresh (don't reuse stale pages).
- **Frame content**: Some myNTU pages use iframes. May need to navigate to the iframe URL directly.
- **Session timeout**: myNTU sessions expire faster than COOL. Re-check login state if extraction fails.
