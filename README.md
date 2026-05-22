# Bulk Employee Incident / Violation Report (EIVR) Generator

A standalone HTML tool that generates EIVR forms in bulk from the Frequent Violators report — by month, violation type, and disciplinary threshold. No server required, runs entirely in the browser.

## Files

| File | Purpose |
|---|---|
| `bulk_eivr_generator.html` | The main generator (open in any modern browser) |
| `EIVR_TEMPLATE_TARDINESS.docx` | Template for Tardiness violations |
| `EIVR_TEMPLATE_NO_BIO.docx` | Template for No Bio violations |
| `EIVR_TEMPLATE_ABSENCES.docx` | Template for Absence violations |
| `EIVR_TEMPLATE_ADJUSTMENT.docx` | Template for Adjustment violations |

## How to Use

1. Open `bulk_eivr_generator.html` in Chrome, Edge, or Firefox.
2. **Step 1 — Upload FV File:** Select the latest Frequent Violators report (e.g., `Frequent_Violators_2026_APRIL_v1.xlsx`).
3. **Step 2 — Template:** The matching template is **auto-loaded** from the same folder based on the violation type. You only need to upload manually if you want to override with a custom template (tick the *"Use a different template"* checkbox).
4. **Step 3 — Configure:**
   - **Violation Type:** TARDINESS, NO BIO, ABSENCES, or ADJUSTMENT
   - **Month:** January–December (the month column in the FV file)
   - **Year:** Used in the form's month label
   - **Company Filter:** ALL or specific company (ICC / API / KTI / DBTPI)
   - **Threshold:** Auto-fills based on policy (you can override)
     - Tardiness: 121 mins (policy = >120 mins/month)
     - No Bio: 4 (policy = >3/month)
     - Absences: 1 (per instance)
     - Adjustment: 1 (no policy threshold — user discretion)
   - **Date Filed:** Defaults to today
   - **Default Immediate Manager:** Leave blank if filling manually
5. **Step 4 — Preview:** Click *Preview Filtered Employees* to see who'll get a form
6. **Generate:** Click *Generate All Forms (ZIP)* — downloads a ZIP organized by company, one DOCX per employee

## Merge Fields

The templates use French quotation marks (« ») as merge delimiters. The generator fills these fields:

| Field | Source |
|---|---|
| `«DATE»` | Date Filed (Step 3) |
| `«EMPLOYEE_NAME»` | NAME / FULL NAME column |
| `«DEPARTMENT»` | DEPARTMENT column |
| `«POSITION»` | POSITION TITLE column |
| `«POSITION1»` | Same as POSITION |
| `«IMMEDIATE_MANAGER»` | Default Manager input (left blank if empty) |
| `«MONTH»` | Selected month + year |
| `«TOTAL»` | Total minutes (Tardiness) or count |
| `«BREAKDOWN»` | Detail breakdown of the violation |

## Output Naming

Files are named: `EIVR_<VIOLATION>_<SURNAME>_<MMMYYYY>.docx`
Example: `EIVR_TARDINESS_ARZAGA_APR2026.docx`

ZIP folder structure:
```
EIVR_TARDINESS_APR2026.zip
├── ICC/
│   ├── EIVR_TARDINESS_MEDRINA_APR2026.docx
│   └── ...
├── API/
│   └── EIVR_TARDINESS_GOMEZ_APR2026.docx
├── KTI/
└── DBTPI/
```

## GitHub Deployment

To host this for the team via GitHub Pages:

1. Create a new GitHub repository (e.g., `infosoft-hr-tools`).
2. Upload all 5 files (HTML + 4 DOCX templates) to the repo root.
3. Go to **Settings → Pages**.
4. Under "Source", select `main` branch and `/ (root)` folder.
5. Save. GitHub will publish the site at `https://<username>.github.io/infosoft-hr-tools/bulk_eivr_generator.html`.
6. Share the URL with the team.

**Note:** Templates can be downloaded directly from the repo by clicking the file → *Download raw file*.

## Browser Requirements

- Chrome, Edge, Firefox, or Safari (last 2 versions)
- Requires internet access for the CDN libraries (xlsx, docxtemplater, pizzip, jszip, FileSaver)
- All processing happens locally in the browser — no data is sent anywhere

## Policy Reference

Based on the Code of Conduct (UPDATES_DISCIPLINARY_MEASURE_FOR_TIMEKEEPING_VIOLATIONS, pages 16–22):

| Violation | Policy Threshold | Type | 1st | 2nd | 3rd | 4th | 5th |
|---|---|---|---|---|---|---|---|
| TARDINESS (mins) | >120 mins/month | A | Written | Final Written | SWP 3 days | SWP 7 days | DISMISSAL* |
| TARDINESS (freq) | >8 instances/month | A | Same | | | | |
| ABSENCES | per instance | A | Same | | | | |
| NO BIO | >3/month | A | Same | | | | |

*DISMISSAL if 5th offense occurs within a 12-month period.

The generator does NOT auto-determine the offense number (1st/2nd/etc.) — HR Specialist must check the box manually based on the employee's disciplinary history.
