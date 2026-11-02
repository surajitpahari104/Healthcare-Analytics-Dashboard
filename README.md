# 🏥 Healthcare Analytics Dashboard | Healthcare KPIs, DAX & Business Intelligence

### Executive Dashboard — Healthcare Performance Analysis

An interactive **Healthcare Analytics Dashboard** developed using Microsoft Power BI, Power Query, data modeling, and DAX (Data Analysis Expressions).

The Executive Dashboard provides an overview of patient activity, appointment performance, hospital operations, departmental efficiency, and healthcare service quality.

---

## 📊 Executive Dashboard Overview

The dashboard provides a centralized view of key healthcare performance indicators, helping users analyze patient trends, appointments, no-show rates, waiting times, and departmental performance.

### Key Performance Indicators

| KPI | Dashboard Display |
|---|---|
| Total Patients | 2K |
| Total Appointments | 5K |
| No-show Rate | 6.8% |
| Average Waiting Time | 23.50 |

*The values above are examples from the dashboard screenshot. Actual results depend on the underlying data and active filters.*

### Technologies Used

- Microsoft Power BI Desktop
- DAX (Data Analysis Expressions)
- Power Query
- Data Modeling
- Interactive Data Visualization
- Time Intelligence Calculations

---

## 🗂️ Data Model

The Executive Dashboard uses the following tables:

**1. Healthcare_Data**

Contains healthcare-related information, including patient records, appointments, departments, doctors, diagnoses, waiting times, treatment outcomes, and operational data.

**2. Date_Table**

Supports date-based analysis and time-intelligence calculations, including month-over-month performance comparisons.

The dashboard uses measures from both tables. DAX measures may reference columns or measures from other tables regardless of their home table.

---

# 🧮 DAX Measures and Equations

## 1. Total Patients Trend

**Measure:** `Total Patients Trend Final`

**Purpose:** Calculates the percentage change in total patients compared with the previous month.

```dax
Total Patients Trend Final =
VAR CurrentMonthValue =
    [Total Patients]

VAR PrevMonthValue =
    CALCULATE(
        [Total Patients],
        DATEADD('Date_Table'[Date], -1, MONTH)
    )

VAR ChangePercent =
    DIVIDE(
        CurrentMonthValue - PrevMonthValue,
        PrevMonthValue
    )

RETURN
    IF(
        ISBLANK(ChangePercent),
        BLANK(),
        IF(
            ChangePercent >= 0,
            "▲ ",
            "▼ "
        )
        & FORMAT(
            ABS(ChangePercent),
            "0.0%"
        )
        & " vs. previous month"
    )
```

**Mathematical equation:**

Monthly Change (%) = ((Current Month Patients − Previous Month Patients) / Previous Month Patients) × 100

An upward arrow represents patient growth, while a downward arrow represents a decrease.

---

## 2. Total Appointments Trend

**Measure:** `Total Appointments Trend Final`

**Purpose:** Measures the month-over-month percentage change in healthcare appointments.

```dax
Total Appointments Trend Final =
VAR CurrentMonthValue =
    [Total Appointments]

VAR PrevMonthValue =
    CALCULATE(
        [Total Appointments],
        DATEADD('Date_Table'[Date], -1, MONTH)
    )

VAR ChangePercent =
    DIVIDE(
        CurrentMonthValue - PrevMonthValue,
        PrevMonthValue
    )

RETURN
    IF(
        ISBLANK(ChangePercent),
        BLANK(),
        IF(
            ChangePercent >= 0,
            "▲ ",
            "▼ "
        )
        & FORMAT(
            ABS(ChangePercent),
            "0.0%"
        )
        & " vs. previous month"
    )
```

**Mathematical equation:**

Appointment Growth (%) = ((Current Month Appointments − Previous Month Appointments) / Previous Month Appointments) × 100

This measure helps identify increasing or decreasing appointment activity.

---

## 3. No-show Rate Trend

**Measure:** `No-show Rate Trend Final`

**Purpose:** Analyzes the percentage change in appointment no-show rates compared with the previous month.

```dax
No-show Rate Trend Final =
VAR CurrentMonthValue =
    [No-show Rate]

VAR PrevMonthValue =
    CALCULATE(
        [No-show Rate],
        DATEADD('Date_Table'[Date], -1, MONTH)
    )

VAR ChangePercent =
    DIVIDE(
        CurrentMonthValue - PrevMonthValue,
        PrevMonthValue
    )

RETURN
    IF(
        ISBLANK(ChangePercent),
        BLANK(),
        IF(
            ChangePercent <= 0,
            "▼ ",
            "▲ "
        )
        & FORMAT(
            ABS(ChangePercent),
            "0.0%"
        )
        & " vs. previous month"
    )
```

**Mathematical equation:**

No-show Rate Change (%) = ((Current Month No-show Rate − Previous Month No-show Rate) / Previous Month No-show Rate) × 100

A decrease in the no-show rate is generally considered a positive operational improvement.

**Note:** This equation calculates relative percentage change, not the difference in percentage points.

---

## 4. Average Waiting Time Trend

**Measure:** `Average Waiting Time Trend Final`

**Purpose:** Measures month-over-month changes in average patient waiting time.

```dax
Average Waiting Time Trend Final =
VAR CurrentMonthValue =
    [Average Waiting Time]

VAR PrevMonthValue =
    CALCULATE(
        [Average Waiting Time],
        DATEADD('Date_Table'[Date], -1, MONTH)
    )

VAR ChangePercent =
    DIVIDE(
        CurrentMonthValue - PrevMonthValue,
        PrevMonthValue
    )

RETURN
    IF(
        ISBLANK(ChangePercent),
        BLANK(),
        IF(
            ChangePercent <= 0,
            "▼ ",
            "▲ "
        )
        & FORMAT(
            ABS(ChangePercent),
            "0.0%"
        )
        & " vs. previous month"
    )
```

**Mathematical equation:**

Waiting Time Change (%) = ((Current Month Average Waiting Time − Previous Month Average Waiting Time) / Previous Month Average Waiting Time) × 100

A reduction in average waiting time generally indicates improved healthcare service efficiency.

---

## 📌 Additional Measures Used by the Executive Dashboard

The following measures were identified as references in the Executive Dashboard's Power BI report definition.

| Measure | Dashboard Usage |
|---|---|
| `Total Patients` | Patient KPI, charts and patient distribution |
| `Total Appointments` | Appointment KPI and time-series charts |
| `No-show Rate` | No-show KPI and departmental analysis |
| `Average Waiting Time` | Waiting-time KPI |
| `Available Beds` | Hospital bed availability |
| `Occupied Beds` | Hospital bed utilization |
| `Total Beds` | Hospital capacity |
| `Bed Occupancy Rate` | Occupancy gauge |
| `Minimum Rate` | Occupancy gauge reference |
| `Maximum Rate` | Occupancy gauge reference |
| `Male Patients` | Patient age and gender analysis |
| `Female Patients Negative` | Patient population chart |
| `LOS Percentage` | Length-of-stay distribution |
| `Business Insight 1–6` | Executive insight cards |

The exact DAX expressions for these pre-existing measures are not reproduced here because their definitions were not recoverable from the accessible report definition. They should be exported from the Power BI semantic model before being documented as source code.

---

# 📈 Executive Dashboard Visualizations

### 1. KPI Summary Cards

Four primary KPI cards summarize:

- Total Patients
- Total Appointments
- No-show Rate
- Average Waiting Time

Each KPI includes a separate month-over-month trend measure with directional indicators.

### 2. Monthly & Yearly Trends

A line chart compares patient and appointment activity across months.

**Measures used:**
- `Total Patients`
- `Total Appointments`

**Time dimension:** `Date_Table[Month]`

### 3. Department-wise Analysis

A horizontal bar chart compares patient counts across hospital departments.

**Measure used:** `Total Patients`

**Category:** Department

### 4. Gender Distribution

A doughnut chart displays the distribution of patients by gender.

**Fields used:**
- `Healthcare_Data[Gender]`
- `Healthcare_Data[Patient_ID]`

A separate Total Patients card can be overlaid at the center of the doughnut chart.

### 5. Diagnosis and Appointment Analysis

A line chart analyzes appointment counts by diagnosis over time.

### 6. Admissions and Discharges

A clustered column chart compares monthly admissions and discharges.

### 7. Insurance Distribution

A doughnut chart displays patient distribution by insurance type.

### 8. Hospital Bed Occupancy

A gauge and supporting KPI cards display hospital bed capacity and occupancy performance.

### 9. Department and Doctor Performance

Charts and tables analyze patient counts, waiting times, no-show rates, and patient satisfaction across departments and doctors.

### 10. Patient Age and Length-of-Stay Analysis

Additional charts summarize age-group distribution and length-of-stay percentages.

---

# 🎛️ Interactive Filters

The Executive Dashboard includes interactive slicers for:

- Appointment Date
- Department
- Doctor
- Gender
- Appointment Status
- Insurance Type

These slicers support interactive analysis across healthcare metrics.

---

# ⚙️ DAX Time Intelligence

The trend measures use the `DATEADD()` function:

```dax
DATEADD(
    'Date_Table'[Date],
    -1,
    MONTH
)
```

This shifts the current date context back by one month.

The `CALCULATE()` function evaluates the selected KPI measure under that shifted date context.

The `DIVIDE()` function performs safe division and returns a blank result when the denominator is zero or blank, unless an alternative result is specified.

The `FORMAT()` function converts the calculated decimal value into a formatted percentage string.

**Requirements:**

- `Date_Table[Date]` must be a valid date column.
- The date table should contain a continuous date range.
- An appropriate relationship must exist between the date table and the relevant healthcare records.
- Month-over-month results require a meaningful date filter context.

---

# 🎨 Dashboard Design

The Executive Dashboard uses a healthcare-themed visual design featuring:

- Blue and white dashboard layout
- Light-blue KPI cards
- Green trend indicators
- Interactive analytical charts
- Department and date filters
- Sidebar navigation
- Executive performance summaries

The trend measures produce directional arrows and formatted text. Green or red text colors are configured separately in Power BI visual formatting.

---

# 💡 Analytical Applications

The dashboard can support:

1. Monitoring healthcare patient activity.
2. Analyzing monthly appointment growth.
3. Identifying changes in appointment no-show rates.
4. Tracking average patient waiting times.
5. Comparing departmental patient volumes.
6. Reviewing bed occupancy and operational capacity.
7. Exploring patient demographics and healthcare service patterns.

---

# 📁 Project Files

```text
Healthcare-Analytics-Dashboard/
│
├── README.md
│
├── Healthcare_Analytics_Dashboard.pbix
│
└── screenshots/
    └── executive_dashboard.png
```

*The directory structure above is a suggested GitHub repository layout. Add the corresponding files before publishing.*

---

# 🚀 How to Use

1. Download the `.pbix` file from the repository.
2. Open it using Microsoft Power BI Desktop.
3. Review and configure the data source if necessary.
4. Refresh the dataset.
5. Navigate to the Executive Dashboard page.
6. Use the available slicers to explore healthcare performance.
7. Review the KPI cards, trend indicators, and analytical visualizations.

---
# 📊 Healthcare Analytics Dashboard — Executive Overview

<img width="1645" height="853" alt="image" src="https://github.com/user-attachments/assets/782d9f74-5445-4d1a-8912-1dae4e69f006" />

---

# 📝 Documentation Scope

This README documents only the **Executive Dashboard** page.

It includes the four final DAX trend formulas and the names of additional measures referenced by Executive Dashboard visuals. The underlying DAX definitions of those additional measures must be verified separately from the semantic model.

Other report pages, including Patient Demographics, Appointments, Department Analysis, and Doctor Performance, are outside the scope of this README.

---

## 📄 Project Summary

**Healthcare Analytics Dashboard** is a Power BI business intelligence project focused on healthcare performance monitoring, interactive reporting, DAX-based KPI calculations, and operational analytics.

The Executive Dashboard consolidates patient, appointment, departmental, and hospital-capacity information into a single analytical interface.

**Developed using:** Microsoft Power BI, DAX, Power Query, and Data Modeling.
