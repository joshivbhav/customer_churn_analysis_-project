# Customer Churn Analysis

An end-to-end customer churn analysis project using Python, SQLite, and exploratory data analysis (EDA). The project combines customer, subscription, and support data to measure churn, identify high-risk segments, and produce actionable retention insights.

## Project Objectives

- Calculate the core customer retention and revenue KPIs.
- Identify churn patterns by plan type, geography, and cancellation month.
- Evaluate support escalations and churn-risk scores as early-warning signals.
- Prepare pivot tables and visualizations for stakeholder reporting.

## Dataset

The SQLite database contains three related tables joined using `customerid`:

| Table | Description | Key fields |
|---|---|---|
| `db_customer` | Customer profile and demographic data | Country, state, gender, date of birth |
| `db_subscription` | Subscription lifecycle and commercial data | Plan, contract, monthly charges, CLTV, cancellation date, churn score |
| `db_support` | Customer support interactions | Complaint date, escalation status, CSAT score |

## Data Preparation

The analysis performs the following preparation steps:

- Renames `name` to `customer_name`.
- Removes sparse or unused columns such as `interests`, `pincode`, `col_1`, and `comment`.
- Converts subscription, cancellation, renewal, complaint, and birth-date fields to date data types.
- Standardizes gender values (`Men` to `Male`, `Women` to `Female`).
- Handles missing country values using geographic context.
- Creates `churn_flag`, where a customer is marked as churned when `cancellation_date` is available.
- Counts support complaints per customer and keeps the most recent support record before merging to prevent duplicate customer rows.
- Creates `tenure_days` and a `churn_risk` band based on churn score:
  - **Low:** score below 50
  - **Medium:** score from 50 to 69
  - **High:** score of 70 or above

## KPIs

| KPI | Definition |
|---|---|
| Churn Rate | Percentage of customers with `churn_flag = 1` |
| Retention Rate | `100 - churn rate` |
| ARPU | Average monthly charges per customer |
| Revenue at Risk | Total monthly charges from churned customers |
| Average Tenure | Mean customer tenure in days |
| Escalation Rate | Percentage of customers with an escalated support record |
| Average Complaints per User | Total complaint count divided by unique customers |

## Analysis Highlights

- **21 customers** are included in the subscription-level analysis.
- **7 customers churned**, producing a churn rate of **33.33%**.
- The **Basic plan** has the highest churn rate (**60.00%**), compared with Standard (**22.22%**) and Premium (**14.29%**).
- The analysis evaluates churn trends over time, churn by state, and correlations among plan tier, contract type, churn score, churn-risk band, escalations, and churn.
- Because the dataset is small, all segment-level findings should be validated with a larger production extract before business policy decisions are made.

## Pivot Table

The plan-level pivot table aggregates customer volume, total monthly charges, and average churn rate.

| Plan Type | Churn Rate | Customers | Monthly Charges (Rs) |
|---|---:|---:|---:|
| Basic | 60.00% | 5 | 52.95 |
| Standard | 22.22% | 9 | 123.91 |
| Premium | 14.29% | 7 | 218.93 |

## Visualizations

The notebook creates the following charts:

1. Monthly churn trend line chart
2. Churn rate by plan type bar chart
3. Churn rate by state bar chart
4. Correlation heatmap for encoded churn-related features
5. Pair plot for numerical and encoded features
6. Category plot comparing plan type, monthly charges, gender, and churn-risk band

## Tools and Technologies

- Python
- Pandas and NumPy for data processing
- SQLite for relational data storage and SQL querying
- Matplotlib and Seaborn for visualization
- Jupyter Notebook for analysis and documentation

## Repository Structure

```text
├── churn_analysis.ipynb                  # Main analysis notebook
├── customer_churn.db                     # SQLite source database
├── exported_churn_data.csv               # Prepared analysis dataset
├── customer_churn_analysis_project_report.docx
├── customer_churn_analysis_project_report.pdf
└── README.md
```

## How to Run

1. Clone this repository.
2. Install dependencies:

   ```bash
   pip install pandas numpy matplotlib seaborn
   ```

3. Open `churn_analysis.ipynb` in Jupyter Notebook or VS Code.
4. Ensure `customer_churn.db` is in the same folder as the notebook.
5. Run the notebook cells in sequence.

## Recommendations

- Target Basic-plan customers with onboarding, pricing/value messaging, and renewal interventions.
- Use high churn scores as a prioritization signal for proactive outreach.
- Trigger service-recovery workflows after escalated support events.
- Monitor churn, retention, ARPU, revenue at risk, tenure, and plan-level performance monthly.
- Expand the dataset and test predictive churn models using train/test validation before deployment.


*This project is intended for learning and portfolio purposes. Results are based on the supplied sample data.*
