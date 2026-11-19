# DAX for the Smoking and Health Analysis report

Table name used below: **`Smoking Health`** (rename in the code if yours differs).
Load `data/Smoking_Health_Analysis.csv`. The file `Smoking_Health_Analysis_with_calculated_columns.csv` holds the expected results so you can check your DAX.

## Objective 2 · Calculated columns
Modeling → New column (one at a time).

```DAX
Age Group =
SWITCH (
    TRUE (),
    'Smoking Health'[Age] <= 29, "18-29",
    'Smoking Health'[Age] <= 44, "30-44",
    'Smoking Health'[Age] <= 59, "45-59",
    "60+"
)
```

```DAX
BP Risk Category =
SWITCH (
    TRUE (),
    'Smoking Health'[Systolic_BP_mmHg] >= 140
        || 'Smoking Health'[Diastolic_BP_mmHg] >= 90, "High",
    'Smoking Health'[Systolic_BP_mmHg] >= 120
        || 'Smoking Health'[Diastolic_BP_mmHg] >= 80, "Elevated",
    "Lower Range"
)
```
Thresholds are a simplified exploratory rule (High: 140/90 or above; Elevated: 120-139 or 80-89; Lower Range: below 120/80). State this on the report.

Needed for Objective 4 (estimated exposure):
```DAX
Pack Years =
DIVIDE ( 'Smoking Health'[Cigarettes_Per_Day], 20 ) * 'Smoking Health'[Years_Smoking]
```
```DAX
Smoking Exposure Level =
SWITCH (
    TRUE (),
    'Smoking Health'[Smoking_Status] = "Never Smoked", "Never smoked",
    'Smoking Health'[Smoking_Status] = "Former Smoker", "Former (amount not recorded)",
    'Smoking Health'[Pack Years] < 5, "Low",
    'Smoking Health'[Pack Years] < 15, "Moderate",
    "High"
)
```

### Sort order (so charts do not sort alphabetically)
Create sort columns, then Column tools → Sort by column:
```DAX
Age Group Sort =
SWITCH ( 'Smoking Health'[Age Group], "18-29", 1, "30-44", 2, "45-59", 3, 4 )
BP Risk Sort =
SWITCH ( 'Smoking Health'[BP Risk Category], "Lower Range", 1, "Elevated", 2, 3 )
Exposure Sort =
SWITCH ( 'Smoking Health'[Smoking Exposure Level],
    "Never smoked", 1, "Former (amount not recorded)", 2, "Low", 3, "Moderate", 4, 5 )
```

## Objective 1 · KPI measures
```DAX
Total Patients = COUNTROWS ( 'Smoking Health' )

Current Smokers =
CALCULATE ( COUNTROWS ( 'Smoking Health' ), 'Smoking Health'[Smoking_Status] = "Current Smoker" )

Patients With Recorded Organ =
CALCULATE ( COUNTROWS ( 'Smoking Health' ),
    'Smoking Health'[Diagnosed_Organ] <> "No Recorded Organ Condition" )

Patients With Recorded Condition =
CALCULATE ( COUNTROWS ( 'Smoking Health' ),
    'Smoking Health'[Health_Condition] <> "No Diagnosed Condition" )
```
Expected totals: 10,000 / 3,015 / 7,227 / 5,772.

## Supporting measures
```DAX
Current Smoker % = DIVIDE ( [Current Smokers], [Total Patients] )
Recorded Condition % = DIVIDE ( [Patients With Recorded Condition], [Total Patients] )
Avg Cigarettes Per Day = AVERAGE ( 'Smoking Health'[Cigarettes_Per_Day] )
Avg Cholesterol = AVERAGE ( 'Smoking Health'[Total_Cholesterol_mg_dL] )
Avg Systolic BP = AVERAGE ( 'Smoking Health'[Systolic_BP_mmHg] )
Avg Diastolic BP = AVERAGE ( 'Smoking Health'[Diastolic_BP_mmHg] )
Smokers % of Age Group =
DIVIDE ( [Current Smokers], CALCULATE ( [Total Patients], ALLEXCEPT ( 'Smoking Health', 'Smoking Health'[Age Group] ) ) )
```
