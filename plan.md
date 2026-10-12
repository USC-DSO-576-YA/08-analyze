# Worked example: a build spec for the wildfire dashboard

This is the **worked planning example** behind [`BUILD_SPEC.md`](BUILD_SPEC.md). It uses Module 7 wildfire data to show the decisions, columns, component contracts, and checks that make a dashboard prompt buildable. The final, self-contained prompt is `BUILD_SPEC.md`; run that file with Codex in this Module 8 repository to produce `dashboard.html`.

For your team's graded Project Phase 2 work, use this **structure**, not this wildfire content. Your team writes its own `BUILD_SPEC.md` about its own user, data, metrics, architecture, and acceptance checks. Phase 2 also requires reconciled team data and working backend analysis code; a finished interface is the Phase 3 task. This wildfire page is a class example, not a Phase 2 submission.

---

## The draft prompt and its structure

Build a responsive HTML dashboard from the bundled Module 7 wildfire monthly data. Work in this Module 8 repository. You may create `build_dashboard.py`, `dashboard.html`, and supporting HTML/CSS/JavaScript files if needed. **Do not edit** the bundled CSV or the separate four-problem homework. Run the build and the checks below, fix errors, and tell me the exact command used and the file to open.

### 1. Current vision and use cases

**User:** a California analyst reviewing reported wildfire activity by discovery month. **Decision:** identify months and year-to-year differences worth investigating further in the source records. The current monthly CSV and basic Streamlit page require several separate charts to answer that question. A single, clearly labeled HTML page should make the pattern and comparison easier to read without implying that reporting counts prove changes in wildfire risk.

| Use case | User selects | Dashboard shows | User decides |
| --- | --- | --- | --- |
| Compare years | A focus year and a comparison year | Monthly reported-record counts aligned January to December, plus the difference in annual counts | Which months explain the difference and merit closer investigation |
| Examine one year | A focus year | That year's counts, three-month average, month-to-month change, and known reported acres | Which months had unusual reported activity or reported size |

Every feature below serves one of these two use cases. Do not add forecasts, causal claims, risk scores, county maps, or other datasets.

### 2. Data and verified definitions

Read the bundled **`data/monthly_wildfire.csv`** in this repository. It was prepared from the Module 7 cleaned wildfire file with the same monthly definitions as the notebook. Do not silently replace or recompute the monthly table. If the CSV is missing, malformed, or lacks required columns, stop with a clear message naming the problem; do not invent rows or numbers.

Required CSV columns:

| Column | Meaning |
| --- | --- |
| `month` | First day of a calendar month; one row per month in ascending order. |
| `fires` | Number of reported wildfire records discovered in that month. |
| `known_acres` | Sum of final reported `FIRE_SIZE` values for records discovered in that month. This is not distinct land burned during the month. |
| `sizes_known` | Count of records with a known `FIRE_SIZE`. |
| `previous` | Previous calendar month's `fires`; missing for the first month. |
| `change` | Current `fires` minus `previous`; missing for the first month. |
| `running` | Cumulative `fires` since the first month in the full series; does not reset each year. |
| `average_3m` | Mean `fires` over the current and up to two preceding calendar months, calculated over the **full** series before any year filter. |

The bundled example source has **283,285 dated records**, spanning January 1992 through December 2024, or **396 calendar months**. For this source, January 1992 has **92** reported records; 2023 has **8,191** and 2024 has **8,182**. These are checks against the bundled source, not values to hard-code in the page. If the saved CSV disagrees, report the mismatch so the student can inspect the notebook output.

Use the CSV's own `known_acres` and `sizes_known` values for display. Do not assume every future dataset has a known size. Keep the distinction between **record counts** and **reported acres** visible in chart titles, units, and notes.

### 3. Starting view and interactions

On first open, show **2024** as the focus year and **2023** as the comparison year when both exist; otherwise choose the latest year and the nearest earlier available year. Populate both year selectors only from years present in the CSV. The starting view has:

1. A title, a one-sentence purpose, and a compact source/interpretation note.
2. Four summary cards for the focus year: reported records, known reported acres, peak reporting month with its record count, and the annual record-count difference from the comparison year. Show the percentage difference only when the comparison-year count is nonzero.
3. A prominent two-line chart comparing monthly `fires` for the selected years. Align by **calendar month**, not by full date. January must line up with January.
4. A focus-year chart showing monthly `fires` and the precomputed `average_3m` together.
5. Two smaller charts: `change` by month (with zero visible and negative values distinct) and `known_acres` by month. Add short captions explaining what each chart answers.
6. A restrained footer explaining that these are reported records and final reported sizes, not proof of changing fire risk or acres newly burned in each month.

Changing the **focus year** updates its cards and three focus-year charts as well as the comparison chart. Changing only the **comparison year** updates the comparison chart and difference card. If both selectors name the same year, show one line with a clear same-year note instead of two indistinguishable lines. If a year has fewer than 12 rows, show only available months and a visible incomplete-year note; do not fabricate missing observations.

### 4. Metric and visual contracts

| Element | Source and calculation | Filter and correctness rule |
| --- | --- | --- |
| Focus-year reported records | Sum `fires` for selected calendar year | Must equal the CSV sum for that year. |
| Focus-year known reported acres | Sum `known_acres` for selected year | Label as reported acres, never as record count or monthly acres burned. |
| Peak reporting month | Focus-year row with largest `fires`; earliest month wins a tie | Show month name and `fires` count. |
| Difference card | Focus-year `fires` sum minus comparison-year `fires` sum; percent uses comparison-year sum as denominator | Show signed count; display percentage as unavailable when denominator is zero. |
| Year comparison | `fires` for each selected year, indexed by month number 1–12 | Lines align by calendar month; unavailable months have gaps, not zeros. |
| Focus-year trend | Focus-year `fires` and CSV `average_3m` | Do not recalculate the average after filtering; January can include the previous November and December. |
| Monthly change | CSV `change` for focus-year rows | January compares with the previous December when present; first-ever month has no change. |
| Known reported acres | CSV `known_acres` for focus-year rows | Keep its scale and unit separate from record counts. |

### 5. Architecture: pieces that can be checked separately

| Piece | Input | Output | What correct means |
| --- | --- | --- | --- |
| Load and validate monthly data | `data/monthly_wildfire.csv` | Ordered, typed monthly records | Required fields exist; dates parse; months are unique and sorted; numeric values are usable; errors name the failing field. |
| Prepare year summaries | Validated monthly records and selected years | Annual totals, peak month, difference, chart-ready monthly arrays | The totals reconcile to CSV rows; month alignment and missing-month handling follow the contracts above. |
| Render HTML page | Validated records and prepared data | `dashboard.html` | Opens without a server or network, updates the specified elements on selector changes, and keeps accurate labels. |

Keep data validation and metric calculations out of page-layout code where practical. Do not duplicate metric formulas independently in several charts or cards.

### 6. Technical and visual design

Use the existing Python environment: **pandas** to load and validate the CSV, and **Plotly** for interactive charts. Generate a self-contained `dashboard.html` with the chart library, data, CSS, and needed JavaScript embedded; it must work by double-clicking the file with no CDN, API key, or running Streamlit server. A small Python build script may create the HTML. No live data fetch is needed because the monthly table is small.

Make it feel like a finished analytical product. Use a warm off-white page background (`#F7F6F2`), white chart cards, dark ink text (`#18252D`), a rust focus-year series (`#C75B39`), and a teal comparison series (`#167D79`). Use a system sans-serif font so the file needs no font download. Arrange the title and controls above a four-card summary row, one wide comparison chart, then a balanced grid of the remaining charts. On a narrow screen, stack cards and charts without clipping labels. Use generous spacing, a clear typographic hierarchy, readable axes and tooltips, sensible number formatting, and visible focus states with labeled controls for keyboard use. Use color **and** labels or position to distinguish series. Preserve the factual language in the data definitions. Avoid decoration that competes with the evidence.

### 7. Build order

1. Inspect the CSV and confirm its schema, date range, and a few known totals. Stop on an actionable error if it is not ready.
2. Implement the validation and year-summary pieces. Check their outputs on 2023 and 2024 before drawing charts.
3. Build the page shell, selectors, summary cards, and main comparison chart. Open the HTML and verify both selectors.
4. Add the focus-year charts, captions, responsive styling, and accessibility details.
5. Run the acceptance checks and fix mismatches. Report the build command, output path, and any remaining limitation.

### 8. Acceptance checks

- The build completes from this Module 8 repository and creates `dashboard.html`; opening it locally requires no server or network.
- For the bundled example, focus year 2024 shows **8,182** reported records and comparison year 2023 shows **8,191**, so the difference card shows **−9** records. These must be calculated from data, not hard-coded.
- The 2023 total and January 1992 count above reconcile to the monthly CSV. The full-series `running` value in December 2024 is **283,285**.
- Selecting 2023 changes the focus-year cards and charts; changing only the comparison year leaves the focus-year charts unchanged.
- The two-year chart aligns equal calendar months. January's `average_3m` and `change` retain their cross-year meaning from the full monthly series.
- Missing columns produce a helpful error; a same-year comparison and a zero-denominator percentage do not break the page.
- Titles, tooltips, cards, and notes distinguish reported record counts from known reported acres. The page is readable at laptop and phone widths and both selectors work with a keyboard.

When finished, provide the HTML file and a brief account of which checks passed. Do not claim that the visualizations establish a change in wildfire risk.

---

## What to transfer to your team's `BUILD_SPEC.md`

The wildfire example contains the same kinds of decisions graded in Phase 2: a revised user-and-decision vision, use cases tied to features, a specific dashboard view, real data definitions, metric calculations, independent component contracts, fit-for-purpose technology, build order, and concrete checks. Replace **every** wildfire fact with a fact verified from your team's reconciled data. Your team must also merge one canonical cleaned dataset on `main` and implement reusable backend analysis code that agrees with the spec. Before submitting, give a fresh agent the repository and `BUILD_SPEC.md`, note every question or guess, and fix the gaps.
