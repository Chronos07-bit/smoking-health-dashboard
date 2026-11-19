# Power BI build guide (Smoking and Health Analysis)

Use Power BI Desktop (Windows). The web version in this repo shows what the finished report should look like.

## 1. Load and check the data
1. Home → Get data → Text/CSV → `Smoking_Health_Analysis.csv` → **Transform Data**.
2. In Power Query: `Diagnosis_Date` = Date; `Age`, `Cigarettes_Per_Day`, `Years_Smoking`, cholesterol, BP and heart rate = Whole number; `BMI` = Decimal number; the rest = Text.
3. Data check: 10,000 rows, no nulls, no duplicate `Patient_ID`. Rename the query to **Smoking Health**. Close & Apply.

## 2. Add DAX
Add the columns and measures from `DAX_Calculated_Columns_and_Measures.md`. Set Sort by column for Age Group, BP Risk Category and Smoking Exposure Level.

## 3. Report pages
**Page 1: Overview (Objective 1)**
- 4 Card visuals: Total Patients, Current Smokers, Patients With Recorded Organ, Patients With Recorded Condition. Optional cards: Avg Cholesterol, Avg Systolic BP.
- Slicers: Gender, Region, Smoking_Status, Age Group (use Tile or Dropdown style).
- Add a text box: "Educational and exploratory analysis. It does not diagnose or prove that smoking causes any condition."

**Page 2: Smoking patterns and indicators (Objective 3)**
| Question | Visual | Fields |
|---|---|---|
| Avg cigarettes per day by status | Clustered column | Axis: Smoking_Status; Values: Avg Cigarettes Per Day |
| Smoking status distribution | Donut | Legend: Smoking_Status; Values: Total Patients |
| Organ-wise condition distribution | Stacked bar | Axis: Diagnosed_Organ; Legend: Health_Condition; Values: Total Patients (turn on data labels, filter out "No Diagnosed Condition" if you want only recorded conditions) |
| Cholesterol by BP risk | Clustered column | Axis: BP Risk Category; Values: Avg Cholesterol (set axis minimum to 0) |

**Page 3: Demographics and exposure (Objective 4)**
| Question | Visual | Fields |
|---|---|---|
| Current smokers by age group | Clustered column + line | Axis: Age Group; Column: Current Smokers; Line: Smokers % of Age Group |
| Smokers by gender / region (optional) | Bar | Axis: Gender or Region; Values: Current Smokers |
| Conditions across exposure levels | Stacked column | Axis: Smoking Exposure Level; Legend: Diagnosed_Organ; Values: Patients With Recorded Condition |
| Recorded-condition rate by exposure | Column | Axis: Smoking Exposure Level; Values: Recorded Condition % |

Sync slicers across pages: View → Sync slicers.

## 4. Formatting tips
- Theme colours: navy `#0F1B2D`, blue `#1637FF`, orange `#FF9900`, purple `#9B30FF`, red `#E11D48` for smokers.
- One title per page, consistent card sizes, no more than 6 visuals per page, turn on Data labels, remove clutter (gridlines).
- Add a tooltip or text box with the BP and exposure definitions.

## 5. What the data shows (for your insights)
- 10,000 patients; 3,015 current smokers (30.1%); 7,227 with a recorded organ (72.3%); 5,772 with a recorded condition (57.7%).
- Age groups: 18-29 = 1,774; 30-44 = 2,213; 45-59 = 2,279; 60+ = 3,734.
- BP risk: Lower Range 2,500; Elevated 5,053; High 2,447.
- Current smokers average 17.1 cigarettes per day. Average cholesterol: Lower Range 195.4, Elevated 197.2, High 197.9 mg/dL (almost flat).
- Current smokers average 202.5 mg/dL cholesterol vs 194.4 for never smokers.
- Share of patients with a recorded condition is 55-60% at every exposure level: no clear rise with exposure.
- Always describe results as associations, never as proof of cause.

## 6. Submit
Save as `.pbix`, export to PDF (File → Export), and add both to this GitHub repo. Add screenshots to the README if your instructor wants them.
