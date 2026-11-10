# Disney+ Hotstar Subscription Billing & Access Operations Database

**Role:** SQL Developer\
**Domain:** OTT subscription management, billing, and access operations\
**Platform:** Google Colab\
**Database:** SQLite\
**Notebook:**
`SQL_Developer_DisneyPlus_Hotstar_OTT_Subscription_DB_Subhadeep_Dey_(DS2606)(2).ipynb`

## Project Overview

This project demonstrates the design and implementation of a SQL-led
operational database for an OTT streaming subscription platform. It
brings together subscription records, billing attempts, lifecycle
events, registered devices, and access entitlements in a relational
database.

The project follows a practical SQL Developer workflow: prepare and
validate operational data, design a normalized database, consume an
internal API, solve business-oriented SQL problems, manage transactions,
design an automated exception-reporting workflow, and document
professional engineering practices.

The notebook is designed to be executed in **Google Colab**. The initial
environment and data-generation setup is preconfigured; the main
development exercises begin at **"SQL DEVELOPER WORK STARTS HERE."**

> **Dataset and brand note:** The dataset is deterministically generated
> for this project and includes controlled operational anomalies for
> repeatable testing. It is not actual or proprietary Disney+ Hotstar
> customer data. The project title retains the legacy Disney+ Hotstar
> label for the defined data domain; the current India platform branding
> is JioHotstar.

## Objectives

-   Design a relational database for OTT subscription billing and access
    operations.
-   Define primary keys, foreign keys, constraints, composite keys, and
    indexes.
-   Retrieve and modify billing data through HTTP API operations.
-   Write SQL queries for operational reporting, revenue analysis,
    reconciliation, and exception monitoring.
-   Apply joins, aggregations, set operations, subqueries, CTEs, and
    window functions.
-   Demonstrate transaction integrity using `COMMIT` and `ROLLBACK`.
-   Design a daily subscription-exception workflow and SQL report.
-   Define Cron schedules and document Git collaboration and safe
    database-change workflows.
-   Validate the outputs of each activity.

## Technology Stack

  -----------------------------------------------------------------------
  Technology                          Purpose
  ----------------------------------- -----------------------------------
  Python                              Notebook orchestration,
                                      deterministic data generation, API
                                      setup, and validation

  SQLite                              Relational database and SQL
                                      execution

  SQL                                 Database design, querying,
                                      reporting, and transaction
                                      management

  Pandas                              Dataset handling and tabular output

  Requests                            HTTP communication with the
                                      temporary internal API

  Graphviz                            Entity Relationship and workflow
                                      diagrams

  Google Colab                        Execution environment
  -----------------------------------------------------------------------

## Dataset

The notebook generates a deterministic dataset using the fixed random
seed `20260814`. The seven source resources contain:

  Resource                       Records
  -------------------------- -----------
  `users`                            120
  `plans`                              5
  `subscriptions`                    180
  `devices`                          300
  `payment_attempts`               1,250
  `subscription_events`              520
  `access_entitlements`              228
  **Total source records**     **2,603**

The setup also creates three system-managed tables used by the
operational workflows:

-   `subscription_ledger`
-   `subscription_exceptions`
-   `api_ingestion_runs`

The dataset includes controlled cases involving billing, refunds,
cancellations, access entitlements, and other operational exceptions.
These cases make the SQL exercises and their validation results
reproducible.

## Database Design

The database contains **seven core tables**:

  -----------------------------------------------------------------------
  Table                               Purpose
  ----------------------------------- -----------------------------------
  `users`                             User account and location details

  `plans`                             Subscription plan, billing cycle,
                                      price, device limit, and content
                                      tier

  `subscriptions`                     User-plan relationship and
                                      subscription lifecycle status

  `devices`                           Registered devices and device
                                      status

  `payment_attempts`                  Initial and renewal billing
                                      attempts, payment status, and
                                      provider references

  `subscription_events`               Activation, renewal, cancellation,
                                      refund, and expiry events

  `access_entitlements`               Device-level access permissions
                                      associated with subscriptions
  -----------------------------------------------------------------------

### Key design features

-   Primary keys identify records in the core entities.
-   Foreign keys maintain relationships between users, plans,
    subscriptions, payments, events, devices, and entitlements.
-   `access_entitlements` uses a **composite primary key** consisting of
    `subscription_id` and `device_id`.
-   The database enables SQLite foreign-key enforcement.
-   The setup creates **10 practical indexes** to support common
    operational queries.
-   Database constraints restrict invalid status values and enforce
    relevant value ranges.

The notebook validates the ER design, including the seven core tables,
required key markers, composite key, and eight expected relationships.

## Project Workflow

The notebook is organized into eight main sections.

### 1. Initial Setup --- Run Once

The preconfigured setup:

-   Imports the required Python libraries and prepares the project
    folders.
-   Generates the deterministic operational dataset.
-   Starts a temporary internal API for the notebook exercises.
-   Fetches API resources, validates the responses, and saves raw JSON
    data.
-   Creates the SQLite database, schema, constraints, and indexes.
-   Prepares validation and visualization helpers.

**Important:** Run the setup section once using **Runtime → Run all**
and wait for `SETUP COMPLETE`. Do not edit the setup cells. The SQL
Developer workspaces are located below the
`SQL DEVELOPER WORK STARTS HERE` heading.

### 2. Database Design and ER Visualization

Defines and validates the seven core entities and their relationships.
The notebook renders an ER diagram and checks the required tables,
primary keys, foreign keys, composite key, and relationships.

### 3. API Operations

Five exercises demonstrate HTTP operations against the temporary
internal API:

  -----------------------------------------------------------------------
  Exercise                Operation               Purpose
  ----------------------- ----------------------- -----------------------
  `API-Q1`                GET                     Fetch filtered
                                                  successful renewal
                                                  payment attempts

  `API-Q2`                POST                    Create a
                                                  billing-attempt record

  `API-Q3`                PUT                     Replace a complete
                                                  billing-attempt record

  `API-Q4`                PATCH                   Update selected
                                                  billing-attempt fields

  `API-Q5`                GET → DELETE → GET      Delete a record and
                                                  verify that it is no
                                                  longer available
  -----------------------------------------------------------------------

The API is provided by the notebook setup. **Developing or deploying a
separate API service is not required.**

### 4. SQL Developer Questions

Thirteen business-oriented SQL exercises are grouped into three
subsections.

#### 4A. SQL Foundations and Multiple Tables

-   **Q1 --- Largest Successful Renewal Charges:** identify the highest
    successful renewal charges.
-   **Q2 --- Plan-Level Successful Revenue:** calculate successful
    payment volume and revenue by plan.
-   **Q3 --- Reconcile Recent Charges with Usable Active Access:**
    reconcile successful renewal charges with valid access entitlements
    and active devices.
-   **Q4 --- Charged Without Usable Active Access:** identify the latest
    qualifying successful charge where usable active access is missing.

#### 4B. SQL Functions, Set Operations, Subqueries, and CTEs

-   **Q5 --- Refunded Users with Open Exceptions:** identify users
    common to the completed-refund and open-exception sets.
-   **Q6 --- Subscriber Operational Risk Bands:** classify users using
    account and exception-related indicators.
-   **Q7 --- Plans Above Average Revenue:** compare plan revenue against
    average plan revenue.
-   **Q8 --- Net Billing and Access State:** combine successful charges,
    completed refunds, and active access information.

#### 4C. Window Functions

-   **Q9 --- City Revenue Ranking:** rank users by successful revenue
    within each city.
-   **Q10 --- Daily Revenue Running Total and Moving Average:**
    calculate daily revenue, cumulative revenue, and a seven-row moving
    average.
-   **Q11 --- Cancellation Trend and Churn Change:** compare daily
    cancellation counts with the previous cancellation date.
-   **Q12 --- First and Latest Successful Payment:** identify the first
    and latest successful payment for each subscription with successful
    billing history.
-   **Q13 --- Top Exception Ranks per City:** rank users by
    open-exception count within each city while preserving ties.

The exercises cover `JOIN`, `LEFT JOIN`, self-joins, anti-joins,
`WHERE`, `GROUP BY`, `HAVING`, aggregate functions, `CASE`, string and
date functions, `COALESCE`, `INTERSECT`, scalar subqueries, CTEs,
`RANK`, `DENSE_RANK`, `ROW_NUMBER`, `LAG`, running totals, and moving
averages.

### 5. Transaction Management

Two exercises demonstrate explicit transaction control:

-   **T1 --- Commit an Approved Billing Adjustment:** insert an approved
    `+INR 149.00` manual adjustment for subscription `S0005` and
    preserve it using `COMMIT`.
-   **T2 --- Roll Back an Unapproved Billing Adjustment:** insert a
    temporary `-INR 99.00` adjustment for subscription `S0006`, then
    remove it using `ROLLBACK`.

The notebook verifies whether the corresponding ledger records are
present or absent after each transaction.

### 6. Workflow Automation

-   **AUTO-FLOW --- Design the Daily Subscription Exception Process:**
    define a seven-step process covering scheduling, API extraction,
    validation, raw JSON storage, SQLite loading, SQL report generation,
    and logging/monitoring.
-   **AUTO-REPORT --- Write the Daily Subscription Exception Report:**
    group open exceptions by type and severity, calculate counts and
    affected subscriptions, and summarize expected and actual values.
-   **AUTO-CRON --- Write Cron Expressions:** define four five-field
    Cron schedules for recurring workflow execution.

**Scheduling limitation:** The Cron expressions describe schedules for a
persistent scheduling environment. A Google Colab runtime cannot
continue running as a permanent scheduler after the runtime stops.

### 7. Engineering Practices

-   **ENG-GIT --- Git Collaboration Workflow:** document an eight-step
    process from requirement review and feature branching through
    testing, commit, push, pull-request review, and approved merge.
-   **ENG-CHANGE --- Safe Database Change Workflow:** document an
    eight-step controlled database-change process covering impact
    analysis, backup, rollback planning, migration preparation, testing,
    peer review, deployment, monitoring, and rollback when required.

These exercises validate the sequence and concepts and render workflow
diagrams. They do not create a remote Git repository or execute a
production database deployment.

### 8. Progress Check

The final dashboard summarizes all **26 activity IDs** and reports their
status as `Passed` or `Needs Review`.

The notebook's recorded final output shows:

  Status           Activities
  -------------- ------------
  Passed                   26
  Needs Review              0
  **Total**            **26**

## How to Run

1.  Open the notebook in Google Colab.
2.  Select **Runtime → Run all**.
3.  Allow the initial setup to finish and confirm that `SETUP COMPLETE`
    is displayed.
4.  Scroll to **SQL DEVELOPER WORK STARTS HERE**.
5.  Work through the sections in notebook order.
6.  Edit only the cells marked **SQL DEVELOPER WORKSPACE**. Leave the
    prebuilt setup and helper cells unchanged.
7.  Execute each exercise and review its output and validation checks.
8.  Confirm that the final Progress Check dashboard reports all
    activities as `Passed`.

The notebook is designed to create its runtime project directory and
SQLite database automatically. In Google Colab, the database is created
at:

``` text
/content/disneyplus_hotstar_subscription_ops_runtime/database/subscription_billing_access_ops.db
```

The setup also creates folders for raw JSON data, processed data,
database files, and outputs under the project runtime directory.

## Validation Approach

Validation is performed throughout the notebook rather than only at the
end. Depending on the activity, checks cover:

-   API response status codes and returned payloads.
-   Required database tables, fields, keys, and relationships.
-   SQL output columns, expected row counts, ordering, and stable
    results.
-   Transaction persistence after `COMMIT` and absence after `ROLLBACK`.
-   Required workflow step counts and concept ordering.
-   Cron expression correctness.
-   Completion status of all 26 required activities.

A `Passed` result indicates that the corresponding notebook validation
checks were satisfied for the generated dataset and configured exercise.

## Repository Contents

The primary project artifact is the Google Colab Jupyter notebook:

``` text
.
└── SQL_Developer_DisneyPlus_Hotstar_OTT_Subscription_DB_Subhadeep_Dey_(DS2606)(2).ipynb
```

The runtime database, raw JSON resources, processed data, and generated
outputs are created by the notebook during execution; they are not
assumed to be pre-existing repository files.

## Scope and Limitations

-   The data is synthetic and generated for repeatable project
    execution; it is not real platform customer data.
-   The API is a temporary local service started by the notebook for
    practice and demonstration.
-   The database engine used is SQLite; SQL syntax and behavior may
    differ from production database systems.
-   Cron expressions are prepared and validated, but the notebook does
    not provide a persistent scheduler.
-   Git and database-change sections document and validate workflows;
    they do not perform remote repository collaboration or production
    deployment.

## Author

**Subhadeep Dey**

------------------------------------------------------------------------

*This project is an educational SQL Developer demonstration covering
relational database design, API operations, SQL analytics, transaction
management, workflow automation, and database engineering practices.*
