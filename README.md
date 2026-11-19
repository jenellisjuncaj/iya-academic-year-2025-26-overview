# Academic Year 2025–26 Enrollment Overview

Standalone IYA dashboard covering Fall 2025 (20253), Spring 2026 (20261), Summer 2026 (20262).

## Publish — one stage at a time

1. Create a NEW blank GitHub repository: iya-academic-year-2025-26-overview.
2. Extract the project ZIP. Upload the files INSIDE academic-year-2025-26-overview to the repository root. index.html must appear directly in the main file list.
3. Preserve these paths: scripts/refresh-data.js and .github/workflows/refresh-data.yml. If files were flattened, edit each file's name to include its correct folder path, then commit. Root files include app.js, overview.js, data.json, sources.json, styles.css, tests.js, netlify.toml, README.md and .nvmrc.
4. Actions → Refresh 2025-26 historical snapshots → Run workflow → main. Wait for a green check. Check semester status on the dashboard for import errors as well.
5. Netlify → Add new project → Import an existing project → GitHub → select the new repository. If missing, configure the Netlify GitHub app and add this repo to its allowed list.
6. Project name: iya-academic-year-2025-26. Branch: main. Base directory: blank. Build command: node scripts/refresh-data.js. Publish directory: a single period (.). Node 22. No environment variables or dependencies needed.
7. Deploy, open the URL, verify all three semester headings and annual total, test View program and course search.

## Refresh and historical limitations

The importer retrieves all nine term/program combinations from https://classes.usc.edu/api/Courses/CoursesByTermSchoolProgram using school=ACAD, program=ACAD/IDSN/PRIN and termCode above. Empty PRIN lists in spring and summer are valid, not failures. Rows are deduplicated under the scheduled program; courses scheduled by other USC schools are out of scope. Each unique identifier is termCode:sectionNumber. Cancelled, missing-count and nonpositive-capacity rows are excluded. Source metadata is validated before replacing a semester; failures preserve a labeled saved snapshot. Annual totals require all three semesters.

These are historical Schedule of Classes snapshots currently published by USC, NOT certified census-date counts or unique student headcounts. Retrieval time does not establish when USC last changed a historical record. The workflow refreshes daily at 15:25 UTC, plus manual runs; Netlify also imports during builds. Public GitHub scheduled workflows can be disabled after extended inactivity; check Actions if retrieval dates stop advancing.

SCH = enrolled × fixed published units, only for sections with exactly one unit value. Variable and unknown units are omitted from SCH but remain in enrollment totals. Known zero-unit courses contribute zero. Initially, 34 sections representing 70 course enrollments are omitted from SCH. Annual utilization uses combined enrollment / combined total seat capacity; not an average of percentages. Available Seats = sum of max(capacity - enrollment, 0).

## Verified initial snapshot

| Metric | Fall | Spring | Summer | Year |
|---|---:|---:|---:|---:|
| Students Enrolled (course registrations) | 1444 | 1400 | 55 | 2899 |
| SCH / units | 4670 | 4870 | 99 | 9639 |
| Sections | 59 | 68 | 11 | 138 |
| Available Seats | 413 | 603 | 56 | 1072 |
| Capacity | 1857 | 2003 | 111 | 3971 |

62 distinct prefix+number course rows. Course suffixes are retained. PRIN has 28 enrollments in fall and no returned scheduled offerings in spring/summer. No individual student records are used.

## Checks

Run node tests.js (Node 22+, no install). Tests check source totals against rows, course aggregation across three semesters, unique term-section identifiers, missing summer blocking annual totals, unit omissions, freshness and failed-import retention. All four program options were checked; desktop browser layout and PRIN filtering were verified. Responsive horizontal scrolling supports the wider three-semester table.

A double-clickable static preview is supplied separately as Academic_Year_2025_26_Preview.html; it contains the initial snapshot and does not refresh itself.
