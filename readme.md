# Smoking and Health Analysis Dashboard

Healthcare analytics project (Sapphire IQ) by Arka Bhattacharya. Explores patient smoking habits, health conditions, cholesterol and blood pressure across age groups, gender and region.

**Live dashboard:** `https://chronos07-bit.github.io/smoking-health-dashboard/`

> Educational and exploratory analysis. It does not provide medical diagnoses and does not prove that smoking causes any health condition.

## Contents
- `index.html` – interactive web dashboard (HTML, CSS, JavaScript, Chart.js; data embedded).
- `powerbi/DAX_Calculated_Columns_and_Measures.md` – DAX for Age Group, BP Risk Category, exposure level and all KPI measures.
- `powerbi/PowerBI_Build_Guide.md` – step-by-step guide to build the same report in Power BI Desktop.
- `data/` – original CSV and a version with the calculated columns for checking your DAX.

## Objectives covered
1. **KPIs:** total patients 10,000; current smokers 3,015; recorded organ category 7,227; recorded health condition 5,772.
2. **Calculated columns:** Age Group (18-29, 30-44, 45-59, 60+) and BP Risk Category (Lower Range, Elevated, High).
3. **Smoking patterns:** cigarettes per day by status, organ-wise conditions, smoking status mix, cholesterol by BP risk.
4. **Demographics:** current smokers by age group; recorded conditions across estimated smoking exposure (pack-years).

## Key findings
- 30.1% of patients are current smokers; they average 17.1 cigarettes per day.
- Cholesterol is nearly identical across BP risk categories (195-198 mg/dL). Current smokers average 202.5 mg/dL vs 194.4 for never smokers.
- The share of smokers is similar in every age group (about 29-31%).
- 55-60% of patients have a recorded condition at every exposure level: no clear increase with exposure.

## Definitions
- Recorded organ = Diagnosed_Organ is not "No Recorded Organ Condition". Recorded condition = Health_Condition is not "No Diagnosed Condition".
- BP High: systolic ≥ 140 or diastolic ≥ 90; Elevated: systolic ≥ 120 or diastolic ≥ 80; otherwise Lower Range.
- Pack-years = cigarettes per day ÷ 20 × years smoked (current smokers). Low < 5, Moderate 5 to < 15, High ≥ 15. Former smokers have no daily amount recorded, so they are shown separately.

## Publish
Settings → Pages → Deploy from a branch → `main` / `(root)`. Keep the file named exactly `index.html`.
