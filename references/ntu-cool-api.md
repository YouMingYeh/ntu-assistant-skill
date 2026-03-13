# NTU COOL Canvas REST API Reference

NTU COOL runs on Canvas LMS. Base URL: `https://cool.ntu.edu.tw`

All API calls use the browser's authenticated session via `evaluate_script` with `fetch()` and `credentials: 'include'`.

## Authentication

No API token needed — use the browser's session cookies:

```javascript
// Template for all API calls via evaluate_script
const response = await fetch('https://cool.ntu.edu.tw/api/v1/ENDPOINT', {
  credentials: 'include',
  headers: { 'Accept': 'application/json' }
});
const data = await response.json();
JSON.stringify(data);
```

If `response.status === 401` or response is HTML (redirect to login), the session has expired.

## Endpoints

### 1. Active Courses
```
GET /api/v1/courses?enrollment_state=active&per_page=50
```
Returns array of course objects:
```json
[{
  "id": 61499,
  "name": "巨量資料系統 Big Data Systems",
  "course_code": "CSIE5043",
  "enrollment_term_id": 123,
  "teachers": [{"display_name": "廖世偉"}],
  "enrollments": [{"type": "StudentEnrollment", "enrollment_state": "active"}]
}]
```

**evaluate_script snippet:**
```javascript
const r = await fetch('https://cool.ntu.edu.tw/api/v1/courses?enrollment_state=active&per_page=50', {credentials:'include', headers:{'Accept':'application/json'}});
const d = await r.json();
JSON.stringify(d.map(c => ({id:c.id, name:c.name, code:c.course_code})));
```

### 2. Announcements (per course)
```
GET /api/v1/courses/:course_id/discussion_topics?only_announcements=true&per_page=30
```
Returns array:
```json
[{
  "id": 12345,
  "title": "Week 3 公告",
  "message": "<p>HTML content...</p>",
  "posted_at": "2026-03-10T10:00:00Z",
  "author": {"display_name": "廖世偉"}
}]
```

**evaluate_script snippet:**
```javascript
const r = await fetch('https://cool.ntu.edu.tw/api/v1/courses/COURSE_ID/discussion_topics?only_announcements=true&per_page=30', {credentials:'include', headers:{'Accept':'application/json'}});
const d = await r.json();
JSON.stringify(d.map(a => ({title:a.title, message:a.message, date:a.posted_at})));
```

### 3. Modules & Materials (per course)
```
GET /api/v1/courses/:course_id/modules?include[]=items&per_page=50
```
Returns modules with nested items:
```json
[{
  "id": 100,
  "name": "Week 1",
  "position": 1,
  "items": [{
    "id": 200,
    "title": "Lecture Slides",
    "type": "File",
    "content_id": 300,
    "url": "https://cool.ntu.edu.tw/api/v1/courses/.../modules/items/200",
    "external_url": "https://..."
  }]
}]
```

Item types: `File`, `Page`, `ExternalUrl`, `ExternalTool`, `Assignment`, `Quiz`, `Discussion`

**evaluate_script snippet:**
```javascript
const r = await fetch('https://cool.ntu.edu.tw/api/v1/courses/COURSE_ID/modules?include[]=items&per_page=50', {credentials:'include', headers:{'Accept':'application/json'}});
const d = await r.json();
JSON.stringify(d.map(m => ({name:m.name, items:m.items?.map(i => ({title:i.title, type:i.type, content_id:i.content_id, url:i.external_url||i.url}))})));
```

### 4. Assignments (per course)
```
GET /api/v1/courses/:course_id/assignments?order_by=due_at&per_page=50
```
Returns:
```json
[{
  "id": 378948,
  "name": "HW 1 - Design Thinking",
  "description": "<p>HTML...</p>",
  "due_at": "2026-03-30T16:01:00Z",
  "points_possible": 100,
  "submission_types": ["online_upload"],
  "html_url": "https://cool.ntu.edu.tw/courses/56440/assignments/378948",
  "has_submitted_submissions": false
}]
```

**evaluate_script snippet:**
```javascript
const r = await fetch('https://cool.ntu.edu.tw/api/v1/courses/COURSE_ID/assignments?order_by=due_at&per_page=50', {credentials:'include', headers:{'Accept':'application/json'}});
const d = await r.json();
JSON.stringify(d.map(a => ({name:a.name, due:a.due_at, points:a.points_possible, url:a.html_url, submitted:a.has_submitted_submissions})));
```

### 5. Grades / Enrollment (per course)
```
GET /api/v1/courses/:course_id/enrollments?user_id=self&include[]=grades
```
Returns:
```json
[{
  "enrollment_state": "active",
  "grades": {
    "current_score": 85.5,
    "current_grade": "A",
    "final_score": null,
    "final_grade": null
  }
}]
```

### 6. Files (per course)
```
GET /api/v1/courses/:course_id/files?per_page=50
```
Returns:
```json
[{
  "id": 500,
  "display_name": "lecture01.pdf",
  "filename": "lecture01.pdf",
  "content-type": "application/pdf",
  "size": 1048576,
  "url": "https://cool.ntu.edu.tw/files/500/download?download_frd=1",
  "created_at": "2026-02-24T08:00:00Z",
  "updated_at": "2026-02-24T08:00:00Z"
}]
```

To get a single file's download URL:
```
GET /api/v1/courses/:course_id/files/:file_id
```
The `url` field contains a time-limited signed download URL.

### 7. User Profile (self)
```
GET /api/v1/users/self/profile
```
Returns current user info (name, login_id, etc.)

## Pagination

Canvas API uses Link header pagination:
- Default: 10 items per page
- Use `per_page=50` (max 100) to reduce requests
- Check `Link` header for `rel="next"` to get more pages

For most NTU courses, 50 items per page is sufficient (< 50 announcements, modules, etc.)

If pagination is needed:
```javascript
async function fetchAll(url) {
  let allData = [];
  let nextUrl = url;
  while (nextUrl) {
    const r = await fetch(nextUrl, {credentials:'include', headers:{'Accept':'application/json'}});
    const d = await r.json();
    allData = allData.concat(d);
    const link = r.headers.get('Link');
    const next = link?.match(/<([^>]+)>;\s*rel="next"/);
    nextUrl = next ? next[1] : null;
  }
  return JSON.stringify(allData);
}
```

## Common Issues

- **401 Unauthorized**: Session expired → prompt user to re-login
- **403 Forbidden**: User not enrolled in course or insufficient permissions
- **422 Unprocessable Entity**: Invalid parameters
- **429 Too Many Requests**: Rate limited → wait 3 seconds, retry
- **HTML response instead of JSON**: Redirected to login page → session expired
- **Empty array `[]`**: No data (valid response, not an error)
