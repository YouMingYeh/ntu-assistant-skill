# NTU ePortfolio Reference

ePortfolio: `https://portal.aca.ntu.edu.tw/eportfolio/`

NTU's academic portfolio system tracks courses, credits, and extracurricular activities.

## URL
- Main: `https://portal.aca.ntu.edu.tw/eportfolio/`
- May redirect to NTU SSO for login

## Important Note
- ePortfolio public profile pages were closed since September 2021
- Student-facing dashboard and course/credit tracking may still be accessible after login
- Functionality may be limited compared to pre-2021

## Authentication
- Uses NTU SSO
- `take_snapshot` after navigation to check login state
- If redirected to SSO login → prompt user to log in

## What to Extract

### Course & Credit Tracking
- Enrolled courses by semester
- Credits earned vs. required (畢業學分)
- Course categories: 必修, 選修, 通識, etc.
- GPA if available

### Activity Records
- Extracurricular activities logged
- Awards, certificates
- Service learning records

## Extraction Approach

1. Navigate to ePortfolio URL
2. `take_snapshot` to check login and available content
3. Look for navigation links to:
   - 修課紀錄 (Course Records)
   - 學分統計 (Credit Statistics)
   - 活動紀錄 (Activity Records)
4. Navigate to each section and `take_snapshot` or `evaluate_script` to extract tables

### evaluate_script example
```javascript
// Extract course/credit tables
const tables = document.querySelectorAll('table');
const data = Array.from(tables).map(t => {
  const rows = Array.from(t.querySelectorAll('tr'));
  return rows.map(r => {
    const cells = Array.from(r.querySelectorAll('td, th'));
    return cells.map(c => c.innerText.trim());
  });
});
JSON.stringify(data);
```

## Output Format
```markdown
## ePortfolio Summary
**Total Credits Earned:** 85 / 128 required

### Credit Breakdown
| Category | Earned | Required |
|----------|--------|----------|
| 必修 (Required) | 45 | 60 |
| 選修 (Elective) | 30 | 48 |
| 通識 (General) | 10 | 20 |
```

## Common Issues
- Service may be partially unavailable (post-2021 changes)
- If pages return 404 or maintenance notice, inform user and skip gracefully
- Fall back to COOL enrollment data if ePortfolio is unavailable
