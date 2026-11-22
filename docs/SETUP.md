# Connecting your own data

The template expects three kinds of source. Each one is set through a Power Query parameter, so you never need to open the Advanced Editor.

## 1. DHIS2 (routine facility data)

| Parameter | What to enter |
| --- | --- |
| `DHIS2_Username` | A DHIS2 account with read access to the data elements and org units you need |
| `DHIS2_Password` | Its password. Power BI stores it in your local credential cache, not in the file you share |

The queries `organisationUnits`, `MNCHN Data` and `Nutrition Data` call the DHIS2 Web API (`/api/analytics` and `/api/organisationUnits`). If your instance uses different data element UIDs, edit the UID list at the top of those queries.

## 2. ODK Central (community component)

| Parameter | What to enter |
| --- | --- |
| `ODK_OData_Url` | The form's OData service root, for example `https://central.example.org/v1/projects/3/forms/community_daily_register.svc` |

Three queries read from it:

- `ODK Submissions`: the main submission table (LGA, community dialogues)
- `ODK Visits`: the `client_visits` repeat (client type, ANC visits, MUAC colour, counselling topics, referral and completion date)
- `ODK Sessions`: the `rpt_sensitization_sessions` repeat (session date, people reached, pregnant women reached)

If your form uses different field names, rename them in the `Table.SelectColumns` step. Missing fields come through as blank rather than breaking the refresh.

## 3. Facility files (.xlsx)

| Parameter | What to enter |
| --- | --- |
| `FacilityList_Url` | Web address of the facility master list workbook |
| `NutritionExtract_Url` | Web address of the facility nutrition extract workbook |

**Facility master list** (one row per facility): Local Government, LGA, Facility Name, Hub Site, Facility Level, Backstop, Supervisor, GPS_Latitude, GPS_Longitude, Estimated Pregnant Women, Estimated children <1 yrs, Estimated children 12 - 23 months, Estimated children 6 - 59 months, Estimated children 12 - 59 months, Estimated Children under 5 years.

**Nutrition extract** (one row per facility per month): reporting month, LGA, facility, then the counselling, birth weight, breastfeeding, MUAC screening, SAM/MAM, deworming, SQ-LNS and vitamin A columns in the order listed in the `Nutrition Facility Data` query.

## Refreshing in the Power BI service

All sources are cloud-reachable, so no on-premises gateway is required. After publishing, open the semantic model settings, enter credentials for each source under *Data source credentials*, and set a scheduled refresh.

## Changing targets

Edit the `KPI Targets` table (KPI, Domain, Target, Direction, Sort, Unit). Status colours, the overall score and the key messages all read from it.
