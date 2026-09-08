# Meridian HR Analytics Dashboard

An end-to-end HR analytics project built in **Power BI**, turning four raw HR data sources into an executive-ready dashboard and a data-driven action plan for company leadership. Covers headcount, attrition, pay, performance, attendance and training across the organisation — with **row-level security** so each manager sees only their own department.

![Executive Overview](images/executive-overview.png)

---

## Project overview

Meridian is a mid-sized company (~320 employees across Sales, Engineering, Customer Service, Operations, Marketing and HR & Finance). Leadership needed a single source of truth for workforce health instead of scattered spreadsheets.

This project ingests four HR datasets, models them into a connected star schema, and presents the results across four dashboard pages tailored to different stakeholders (CEO, CHRO, and line managers). The final layer translates the numbers into a prioritised set of recommended actions using a **WHAT / So What / Now What** storytelling structure.

## Key findings

Three insights surfaced by the dashboard, each with a quantified business impact and a concrete recommendation:

- **Sales attrition is nearly double the company average** — 17.1% vs 9.7%, with 14 of 82 people leaving. At an estimated replacement cost of ~£11,188 per hire, that points to an implied cost of roughly **£156,625**. Sales also records the most "Unsatisfactory" performance ratings, compounding the retention risk.
- **Training certification is missing target** — 61.7% company-wide against a 75% goal, with no department currently hitting it. The average assessment score (73.9%) sits just above the 70% pass mark, suggesting many learners fall short by a small margin — a study-support and readiness gap rather than a motivation one.
- **Customer Service has the weakest attendance** — 94.3% vs a company-wide 95.9%, equating to ~2.5 extra absence days per person. Because it is a customer-facing function, sustained low attendance risks showing up directly in service levels.

Full storytelling and recommended actions are in [`reports/Executive_Presentation.docx`](reports/Executive_Presentation.docx).

## Dashboard pages

| Page | Audience | Focus |
|------|----------|-------|
| Executive Overview | CEO | Headcount, attrition %, total salary cost, avg salary by department and job level |
| Performance & Reward | Leadership | Performance ratings, target achievement, bonus eligibility |
| Attendance & Wellbeing | Line managers | Attendance trends by month/department, leave-type mix, absence per employee |
| Training & Development | CHRO / L&D | Certification rates, assessment scores, course completion |

![Attendance and Wellbeing](images/attendance-and-wellbeing.png)

## Data model

Four CSV sources joined on `employee_id`, modelled as a star schema with `hr_employees` as the dimension table:

| Table | Grain | Rows | Key fields |
|-------|-------|------|------------|
| `hr_employees` | one row per employee | 320 | department, job_level, location, salary_gbp, tenure_years, employment_status |
| `hr_attendance` | employee × month | 3,360 | working_days, days_present, days_absent, leave_type, attendance_pct |
| `hr_performance` | employee × review year | 560 | performance_rating, target_achievement_pct, bonus_eligible |
| `hr_training` | employee × course | 990 | course_name, score_pct, certified, year |

## Row-level security

The report implements RLS so each departmental manager sees only their own team's data. The screenshot below shows the "viewing as" a manager role — the Executive Overview automatically filters to just that manager's department:

![Row-level security demo](images/row-level-security-demo.png)

## Tech stack & skills demonstrated

- **Power BI Desktop** — data modelling, report design, multi-page navigation
- **Power Query (M)** — data cleaning and shaping across four sources
- **DAX** — measures for attrition %, attendance %, certification rate, cost-of-attrition, and target achievement
- **Star-schema data modelling** — relationships across dimension and fact tables
- **Row-Level Security (RLS)** — role-based data access
- **Executive storytelling** — translating metrics into prioritised, costed recommendations

## Repository structure

```
meridian-hr-analytics/
├── README.md
├── data/                       # source datasets (synthetic)
│   ├── hr_employees.csv
│   ├── hr_attendance.csv
│   ├── hr_performance.csv
│   └── hr_training.csv
├── dashboard/
│   └── Meridian_HR_Dashboard.pbix
├── reports/
│   └── Executive_Presentation.docx
└── images/                     # dashboard screenshots
```

## How to explore

1. Download `dashboard/Meridian_HR_Dashboard.pbix` and open it in [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free).
2. The data is embedded, so the report loads without needing to reconnect the CSVs.
3. Use the page tabs along the bottom to move between the four report views.

## Note on the data

All datasets are **synthetic** and generated for portfolio purposes. No real employee data is used, and any names, salaries and identifiers are fictional.
