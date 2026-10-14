# Uber India Driver Earnings & Trip Settlement System

A SQL Developer project focused on analyzing driver earnings, trip records, payout settlements, and payment exceptions using SQL, SQLite, and Python.

The project demonstrates how a relational database can organize operational data, reconcile expected payouts with actual transfers, identify missing records, and generate reports that support business decisions.

The implementation runs in Google Colab and uses a project-provided internal API and a generated Dataset for development and validation.

> **Project Type:** SQL Development, Database Management, and Data Analysis  
> **Role:** SQL Developer  
> **Environment:** Google Colab  
> **Database:** SQLite



## About the Project

In a ride-hailing business, managing driver earnings and payouts requires accurate records of trips, incentives, payout batches, and payment transfers.

When these records are not properly connected, identifying missing transfers, payment mismatches, and unresolved exceptions can become difficult.

This project demonstrates a database-based approach to organizing these records and answering operational questions through SQL.

The main objectives are to:

- Organize driver, trip, and payout information using relational tables.
- Analyze trip records and driver earnings.
- Reconcile expected payout amounts with recorded transfer amounts.
- Identify missing transfers and mismatched payments.
- Track open settlement exceptions.
- Demonstrate database transactions using COMMIT and ROLLBACK.
- Design an automated reporting workflow.
- Validate SQL outputs and engineering workflows.


## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Setup, API consumption, and validation |
| SQL | Business queries and payout analysis |
| SQLite | Relational database management |
| Google Colab | Development and execution environment |
| Requests | HTTP requests to the provided internal API |
| JSON | API request and response data |
| Pandas | Loading and displaying tabular data |
| Graphviz | Visualizing database and workflow diagrams |



## Database Design

The project uses related relational tables to organize operational information.

### Core Tables

| Table | Purpose |

| `drivers` | Stores driver information |
| `commission_rules` | Stores commission-related rules |
| `trips` | Stores trip records and earnings information |
| `incentives` | Stores incentive-related records |
| `payout_batches` | Stores payout batch information |
| `payout_items` | Stores individual payout records |
| `payout_transfers` | Stores payment transfer records |

### System-Managed Tables

| Table | Purpose |
|---|---|
| `driver_ledger` | Maintains driver ledger entries |
| `settlement_exceptions` | Tracks settlement-related exceptions |
| `api_ingestion_runs` | Stores API ingestion metadata |

Primary keys, foreign keys, constraints, and indexes support data integrity and relational querying.

The ER diagram illustrates how the main business entities are connected.



## Key Project Features

### 1. API Integration

The project demonstrates five API activities:

- **GET:** Retrieve operational records.
- **POST:** Create a new record.
- **PUT:** Replace an existing resource.
- **PATCH:** Update selected fields.
- **GET → DELETE → GET:** Verify deletion through HTTP responses.

The deletion workflow expects the status sequence `200 → 200 → 404`.

The internal API is provided by the project setup. This implementation demonstrates API consumption rather than building or deploying a production API server.

### 2. SQL Business Analysis

The project contains 13 SQL activities covering different database concepts.

These queries demonstrate:

- Filtering, sorting, and limiting records.
- Aggregate functions such as `COUNT()` and `SUM()`.
- Grouping and filtering aggregated results.
- INNER JOIN and LEFT JOIN operations.
- Identifying missing matching records.
- Subqueries and Common Table Expressions (CTEs).
- `CASE`, `COALESCE`, and other SQL functions.
- Payout reconciliation and exception analysis.
- Window functions such as `RANK()`, `DENSE_RANK()`, `ROW_NUMBER()`, and `LAG()`.
- Running totals and moving averages.
- First and latest trip analysis.
- Ranking exceptions within cities.

### 3. Payout Reconciliation

An important part of this project is comparing expected payout amounts with recorded transfer amounts.

The reconciliation logic classifies payouts into three categories:

| Classification | Meaning |
|---|---|
| `MATCHED` | Expected payout agrees with the recorded transfer amount |
| `MISSING` | A corresponding transfer record is missing |
| `MISMATCHED` | The recorded transfer amount differs from the expected amount |

These classifications help identify records that may require further investigation.

### 4. Settlement Exception Reporting

The project includes an automated SQL report that groups open settlement exceptions by exception type and severity.

The report returns:

- `exception_type`
- `severity`
- `exception_count`
- `total_expected_value`
- `total_actual_value`

This summary helps operational teams review exception categories and prioritize further investigation.

### 5. Transaction Management

The notebook demonstrates two transaction activities.

**T1 — COMMIT**

An approved manual payout adjustment is inserted into the driver ledger within a transaction. After COMMIT, the record should remain in the database.

**T2 — ROLLBACK**

A temporary, unapproved adjustment is attempted within a transaction. ROLLBACK cancels the change, so the temporary record should not remain.

These activities demonstrate how transactions can help protect data integrity.

### 6. Workflow Automation

The project defines a seven-step automation workflow:

1. Scheduler or cron trigger.
2. Call the internal API.
3. Validate HTTP status and response schema.
4. Store raw JSON.
5. Load validated records into SQLite.
6. Generate the SQL report.
7. Log and monitor run status, row counts, and errors.

The workflow is validated and represented as a flowchart.

### 7. Cron Scheduling

Four scheduling expressions are validated in the notebook.

| Scenario | Cron Expression |
|---|---|
| Daily at 8 AM, 12 PM, and 4 PM | `0 8,12,16 * * *` |
| Every weekday at 5 PM | `0 17 * * 1-5` |
| Every day at midnight | `0 0 * * *` |
| Every Sunday at 2 AM | `0 2 * * 0` |

**Note:** These are scheduling definitions. Google Colab does not provide a permanent scheduler after its runtime stops.

### 8. Engineering Workflows

The project includes two validated engineering workflows.

**Git Collaboration Workflow**

Read ticket → Feature branch → SQL change → Testing and validation → Commit → Push → Pull request and review → Merge after approval.

**Safe Database Change Workflow**

Understand requirement → Backup and rollback plan → Change script → Development testing → Peer review → Approved deployment → Monitoring → Rollback if failure occurs.

Both workflows are represented as flowcharts.


## Validation Results

The project includes 26 validated activities covering database design, API operations, SQL analysis, transactions, automation, scheduling, and engineering workflows.

The final progress dashboard displayed the following results during validation:

| Validation Status | Count |
|---|---:|
| Passed | 26 |
| Needs Review | 0 |
| Total Activities | 26 |

The validation framework checks aspects such as SQL output columns, row counts, expected results, API responses, transaction behavior, cron expressions, and workflow order.

These results reflect the notebook's configured validation checks and the state shown in the completed runtime.



## How to Run the Project

The project is designed to run in Google Colab.

1. Open the project notebook in Google Colab.
2. Execute the setup cells in the required order.
3. Allow the notebook to generate the Dataset and prepare the SQLite database.
4. Complete or review the API activities.
5. Run the SQL queries and inspect their outputs.
6. Execute the transaction exercises.
7. Run the automation report and cron validation.
8. Validate the Git and safe database change workflows.
9. Review the final progress dashboard.

**Important:** Run the setup cells before running dependent activities. The notebook uses a temporary internal API and runtime-managed resources, so restarting the runtime may require setup to be executed again.


## Business Value

This project demonstrates how SQL and relational database concepts can support operational analysis.

Potential business uses include:

- Investigating payout discrepancies.
- Identifying missing transfer records.
- Reviewing driver earnings and trip activity.
- Summarizing open settlement exceptions.
- Ranking drivers and comparing daily earnings.
- Supporting finance and operations teams with structured reports.
- Improving data quality through validation and constraints.
- Demonstrating safe database transaction practices.

The project is a development and learning implementation. Its reports support investigation and decision-making; they do not automatically confirm financial losses or resolve exceptions.


## Limitations

- The implementation runs in Google Colab using SQLite.
- The Dataset and internal API are provided or generated within the project environment.
- The internal API is not deployed as a production service by this project.
- Cron expressions are validated, but a persistent scheduling service is not configured.
- Production deployment, frontend development, cloud infrastructure, and production monitoring are outside the current implementation scope.


## Future Improvements

Possible future improvements include:

- Connecting the project to a persistent database server.
- Integrating a production API with appropriate authentication.
- Deploying scheduled reports through a persistent scheduling service.
- Creating a dashboard for payout and exception monitoring.
- Adding more extensive automated tests and logging.
- Introducing role-based access controls and production security measures.
- Improving reporting performance as the Dataset grows.

These are potential enhancements and are not claimed as implemented features.


## What I Learned

Working on this project helped me understand how SQL development connects to a wider business process.

I practiced relational database design, table relationships, SQL joins, aggregation, subqueries, CTEs, and window functions.

I also worked with API operations, JSON data, SQLite, transaction management, exception reporting, and result validation.

One of my key takeaways is that SQL development is not only about writing queries. It also involves understanding business requirements, maintaining data integrity, investigating exceptions, validating outputs, and presenting information in a useful form.


## Project Information

**Project Name:** Uber India Driver Earnings & Trip Settlement System  
**Role:** SQL Developer  
**Tools:** Python, SQL, SQLite, Google Colab, Requests, JSON, Pandas, and Graphviz  
**Focus Areas:** Database Design, SQL Analysis, API Consumption, Payout Reconciliation, Transactions, Automation, and Validation

This is an independent portfolio project and is not an official Uber product or an indication of affiliation with Uber.
