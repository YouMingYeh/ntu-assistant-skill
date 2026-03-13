# Output Format Specification

All output files are created in the user's current working directory.

## File Structure

```
{current_directory}/
├── COURSE_SUMMARY.md          # Master course summary (primary output)
├── schedule.md                # Weekly timetable from myNTU
├── deadlines.md               # Consolidated deadlines sorted by date
├── {CourseName}/              # Per-course folder (English, underscores)
│   ├── Week{N}/               # Weekly materials (if download opted in)
│   │   ├── lecture_slides.pdf
│   │   └── reading.pdf
│   └── Homework/              # Assignment-related files
│       └── HW1_instructions.pdf
```

## Naming Conventions

- **Folder names**: English, underscores for spaces (e.g., `Cloud_Native_App_Dev/`, `Big_Data_Systems/`)
- **Week folders**: `Week1/`, `Week2/`, etc.
- **File names**: Keep original filenames from COOL when downloading
- **Semester format**: `114-2` (academic year - semester), e.g., 114-2 = 2026 Spring
- **Dates**: Always `YYYY-MM-DD` format

## COURSE_SUMMARY.md Format

```markdown
# NTU COOL - {semester} ({year} {Spring/Fall}) Course Summary
**Student:** {student_id}
**Last updated:** {YYYY-MM-DD}

---

## Course {N}: {course_name_chinese} {course_name_english}
**Course ID:** {canvas_course_id}
**URL:** https://cool.ntu.edu.tw/courses/{id}
**Instructor:** {teacher_name}

### Announcements
1. **{title}** ({YYYY-MM-DD})
   - {summary content}

2. **{title}** ({YYYY-MM-DD})
   - {summary content}

### Modules / Materials
**Week {N} - {M/DD}: {module_title}**
- ✅ `{filename}` (downloaded)
- ⬜ [{title}]({url}) ({type}: Google Slides / PDF / Video)

### Assignments
1. **{assignment_name} ({weight}%)** ⚠️ **DUE: {YYYY-MM-DD}**
   - Submission: {type}
   - URL: {url}

### Grades
- Current Score: {score}
- Current Grade: {letter}

---

## ⚠️ Action Items & Deadlines

| Deadline | Course | Task |
|----------|--------|------|
| {YYYY-MM-DD} | {course} | {task description} |

---

## File Structure
```
(tree view of created directories and files)
```
```

## schedule.md Format

```markdown
# {semester} Weekly Schedule (課表)
**Last updated:** {YYYY-MM-DD}

| Time | Mon | Tue | Wed | Thu | Fri |
|------|-----|-----|-----|-----|-----|
| 1 (08:10-09:00) | | | | | |
| 2 (09:10-10:00) | | | | | |
| 3 (10:20-11:10) | | | | | |
| 4 (11:20-12:10) | | | | | |
| 5 (12:20-13:10) | | | | | |
| 6 (13:20-14:10) | | | | | |
| 7 (14:20-15:10) | | | | | |
| 8 (15:30-16:20) | | | | | |
| 9 (16:30-17:20) | | | | | |
| 10 (17:30-18:20) | | | | | |
| A (18:25-19:15) | | | | | |
| B (19:20-20:10) | | | | | |
| C (20:15-21:05) | | | | | |
| D (21:10-22:00) | | | | | |

Cell format: `{course_short_name} @{classroom}`
```

## deadlines.md Format

```markdown
# {semester} Deadlines
**Last updated:** {YYYY-MM-DD}

## Upcoming

| Date | Course | Assignment | Weight | Status |
|------|--------|-----------|--------|--------|
| {YYYY-MM-DD} | {course} | {name} | {%} | ⬜ Not submitted |
| {YYYY-MM-DD} | {course} | {name} | {%} | ✅ Submitted |

## Past Due

| Date | Course | Assignment | Weight | Status |
|------|--------|-----------|--------|--------|
| {YYYY-MM-DD} | {course} | {name} | {%} | ✅ Submitted |
```

## Update Rules

1. **Check before writing**: Always read existing files first
2. **Merge, don't overwrite**: If COURSE_SUMMARY.md exists, update sections rather than replacing the whole file
3. **Preserve user edits**: If user has added notes or modified content, keep their changes
4. **Update timestamp**: Always update "Last updated" date
5. **Mark downloads**: Use ✅ for downloaded files, ⬜ for pending/link-only
6. **Flag urgency**: Use ⚠️ for deadlines within 7 days, mark "TODAY" for same-day deadlines
