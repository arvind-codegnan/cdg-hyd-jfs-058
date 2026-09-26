## Part 3

## Purpose

This exercise pack contains 10 new standalone-table exercises.

Learners will practise:

- creating tables;
- defining primary-key constraints;
- defining unique constraints;
- applying `NOT NULL` constraints;
- assigning default values;
- defining named `CHECK` constraints;
- inserting single and multiple rows;
- updating individual and multiple rows; and
- deleting rows safely using precise `WHERE` conditions.

## New Tables

1. `courses`
2. `instructors`
3. `warehouses`
4. `shipments`
5. `subscriptions`
6. `events`
7. `devices`
8. `restaurants`
9. `insurance_policies`
10. `job_postings`

## Environment and Safety Rules

- Use meaningful names for constraints, such as `uq_courses_course_code` and `chk_courses_fee`.

## How to Interpret the CHECK Conditions

- MySQL evaluates each enforced `CHECK` condition whenever a row is inserted or updated.
- A checked row is accepted when the expression evaluates to `TRUE` or `UNKNOWN`, and rejected when it evaluates to `FALSE`.
- Therefore, `CHECK (amount > 0)` alone does not prevent `NULL`; use `NOT NULL` as well when the value is mandatory.
- For an optional column, explicitly allow `NULL`, for example: `CHECK (end_date IS NULL OR end_date >= start_date)`.
- MySQL treats `BOOLEAN` as a numeric type, so this pack explicitly uses checks such as `CHECK (is_active IN (0, 1))`.
- Conditions comparing two columns, such as `maximum_salary >= minimum_salary`, should be declared as table-level checks.
- Prefix every check name with its table name because check-constraint names must be unique within the schema.
- Do not use `NOT ENFORCED`; every check in this exercise pack must be enforced.

## Recommended Order for Every Use Case

1. Read the scenario and column requirements.
2. Write and execute the `CREATE TABLE` statement.
3. Inspect the structure using `DESCRIBE` and `SHOW CREATE TABLE`.
4. Complete the valid insert exercises.
5. Attempt the invalid inserts and record the errors.
6. Complete the update exercises.
7. Complete the delete exercises.
8. Verify the final contents of the table.

---

# 1. Online Course Catalogue

**Suggested table:** `courses`

## Scenario

A training institute needs one table containing its independent course catalogue. Instructor assignments are outside the scope of this exercise.

## Column Requirements

| Column | Suggested type | Constraints and rules |
|---|---|---|
| `course_id` | `INT UNSIGNED` | Primary key and auto-increment |
| `course_code` | `VARCHAR(15)` | Required and unique |
| `course_title` | `VARCHAR(150)` | Required |
| `category` | `VARCHAR(60)` | Required |
| `duration_hours` | `DECIMAL(5,1)` | Required; must be greater than `0` |
| `fee` | `DECIMAL(10,2)` | Required; default `0.00`; cannot be negative |
| `delivery_mode` | `VARCHAR(20)` | Required; default `ONLINE`; allow `ONLINE`, `CLASSROOM`, or `HYBRID` |
| `course_status` | `VARCHAR(20)` | Required; default `DRAFT`; allow `DRAFT`, `ACTIVE`, `INACTIVE`, or `ARCHIVED` |
| `created_at` | `TIMESTAMP` | Required; default current timestamp |

## Required CHECK Constraints

| Suggested constraint name | Exact condition | Invalid examples rejected |
|---|---|---|
| `chk_courses_duration` | `duration_hours > 0` | Zero or negative duration |
| `chk_courses_fee` | `fee >= 0` | Negative fee |
| `chk_courses_delivery_mode` | `delivery_mode IN ('ONLINE', 'CLASSROOM', 'HYBRID')` | Any unsupported delivery mode |
| `chk_courses_status` | `course_status IN ('DRAFT', 'ACTIVE', 'INACTIVE', 'ARCHIVED')` | Any unsupported course status |

## A. CREATE Exercises

1. **CRS-C1:** Create `courses` with all specified columns and constraints.
2. **CRS-C2:** Give the unique constraint and each check constraint a meaningful name.
3. **CRS-C3:** Use `DESCRIBE courses` and `SHOW CREATE TABLE courses` to verify the primary key, unique key, defaults, nullability, and checks.

## B. INSERT Exercises

| Course code | Title | Category | Hours | Fee | Delivery mode | Status |
|---|---|---|---:|---:|---|---|
| CRS-JAVA-101 | Java Fundamentals | Programming | 40.0 | 6000.00 | CLASSROOM | ACTIVE |
| CRS-SQL-102 | MySQL Essentials | Database | 32.0 | 4500.00 | ONLINE | ACTIVE |
| CRS-WEB-103 | Responsive Web Design | Web Development | 28.0 | 0.00 | Use default | Use default |
| CRS-TST-104 | Software Testing Basics | Testing | 24.0 | 3500.00 | HYBRID | ACTIVE |
| CRS-OLD-105 | Legacy Systems Overview | Technology | 12.0 | 2000.00 | ONLINE | ARCHIVED |

1. **CRS-I1:** Insert `CRS-JAVA-101` using an explicit column list.
2. **CRS-I2:** Insert `CRS-SQL-102` and `CRS-WEB-103` using one multi-row statement. Omit the two defaulted columns for `CRS-WEB-103` by using a suitable separate statement if necessary.
3. **CRS-I3:** Insert the remaining two courses.
4. **CRS-I4:** Attempt to insert a duplicate course code.
5. **CRS-I5:** Attempt inserts having a `NULL` title, zero duration, negative fee, and delivery mode `SELF_PACED`.

## C. UPDATE Exercises

1. **CRS-U1:** Change the fee of `CRS-JAVA-101` to `6500.00`.
2. **CRS-U2:** Increase the fee of every active course by `10%`.
3. **CRS-U3:** Change `CRS-WEB-103` to `ACTIVE` and set its fee to `2500.00`.
4. **CRS-U4:** Increase the duration of every testing course by `4` hours.
5. **CRS-U5:** Attempt to change a duration to `0` and confirm that the check constraint rejects the update.

## D. DELETE Exercises

1. **CRS-D1:** Preview and delete the archived course `CRS-OLD-105`.
2. **CRS-D2:** Insert a temporary course with code `CRS-TEMP-999` and delete it using the unique course code.
3. **CRS-D3:** Preview and delete courses in the `Testing` category whose fee is below `5000.00`.

---

# 2. Instructor Directory

**Suggested table:** `instructors`

## Scenario

A training organisation needs one table containing instructor employment and payment information.

## Column Requirements

| Column | Suggested type | Constraints and rules |
|---|---|---|
| `instructor_id` | `INT UNSIGNED` | Primary key and auto-increment |
| `instructor_code` | `VARCHAR(12)` | Required and unique |
| `first_name` | `VARCHAR(60)` | Required |
| `last_name` | `VARCHAR(60)` | Required |
| `email` | `VARCHAR(120)` | Required and unique |
| `specialization` | `VARCHAR(100)` | Required |
| `years_experience` | `TINYINT UNSIGNED` | Required; default `0`; range `0` through `50` |
| `hourly_rate` | `DECIMAL(10,2)` | Required; must be greater than `0` |
| `employment_type` | `VARCHAR(20)` | Required; default `PART_TIME`; allow `FULL_TIME`, `PART_TIME`, or `CONTRACT` |
| `is_active` | `BOOLEAN` | Required; default `TRUE` |
| `joined_on` | `DATE` | Required |
| `created_at` | `TIMESTAMP` | Required; default current timestamp |

## Required CHECK Constraints

| Suggested constraint name | Exact condition | Invalid examples rejected |
|---|---|---|
| `chk_instructors_experience` | `years_experience BETWEEN 0 AND 50` | Experience below `0` or above `50` |
| `chk_instructors_hourly_rate` | `hourly_rate > 0` | Zero or negative rate |
| `chk_instructors_employment_type` | `employment_type IN ('FULL_TIME', 'PART_TIME', 'CONTRACT')` | Any unsupported employment type |
| `chk_instructors_active_flag` | `is_active IN (0, 1)` | Boolean values other than `0` or `1` |

## A. CREATE Exercises

1. **INS-C1:** Create `instructors` with a primary key, two unique constraints, defaults, and named checks.
2. **INS-C2:** Ensure that experience cannot exceed `50` years and hourly rate must be positive.
3. **INS-C3:** Inspect the completed definition and identify every column that cannot contain `NULL`.

## B. INSERT Exercises

| Code | First name | Last name | Email | Specialization | Experience | Rate | Employment type | Active | Joined on |
|---|---|---|---|---|---:|---:|---|---|---|
| INS-JV-001 | Kavya | Menon | kavya.menon@example.test | Java | 8 | 1500.00 | FULL_TIME | TRUE | 2021-06-14 |
| INS-DB-002 | Ritesh | Kumar | ritesh.kumar@example.test | Databases | 12 | 1800.00 | CONTRACT | TRUE | 2019-02-01 |
| INS-WB-003 | Farah | Ali | farah.ali@example.test | Web Development | 5 | 1200.00 | Use default | Use default | 2023-08-21 |
| INS-QA-004 | Nitin | Bose | nitin.bose@example.test | Software Testing | 7 | 1350.00 | PART_TIME | TRUE | 2022-01-10 |
| INS-OLD-005 | Leela | Shah | leela.shah@example.test | Mainframe Systems | 25 | 2000.00 | CONTRACT | FALSE | 2010-05-17 |

1. **INS-I1:** Insert the first instructor.
2. **INS-I2:** Insert the next two instructors and allow defaults to populate the omitted values for `INS-WB-003`.
3. **INS-I3:** Insert the remaining two instructors using one multi-row statement.
4. **INS-I4:** Attempt to insert a duplicate instructor code and a duplicate email.
5. **INS-I5:** Attempt inserts with `NULL` specialization, `51` years of experience, zero hourly rate, and employment type `FREELANCE`.

## C. UPDATE Exercises

1. **INS-U1:** Increase `INS-QA-004` hourly rate to `1500.00`.
2. **INS-U2:** Increase the hourly rate of every active contract instructor by `8%`.
3. **INS-U3:** Change the specialization of `INS-WB-003` to `Full-Stack Web Development`.
4. **INS-U4:** Increase the experience of all active instructors by one year without exceeding `50`.
5. **INS-U5:** Deactivate `INS-OLD-005` only if it is not already inactive; observe the affected-row count.

## D. DELETE Exercises

1. **INS-D1:** Preview and delete inactive instructor `INS-OLD-005`.
2. **INS-D2:** Insert an instructor with code `INS-TEMP-99` and then delete only that row.
3. **INS-D3:** Preview and delete active part-time instructors having fewer than `6` years of experience.

---

# 3. Warehouse Capacity Register

**Suggested table:** `warehouses`

## Scenario

A logistics company needs one independent table for warehouse location, capacity, occupancy, facilities, and operating status.

## Column Requirements

| Column | Suggested type | Constraints and rules |
|---|---|---|
| `warehouse_id` | `INT UNSIGNED` | Primary key and auto-increment |
| `warehouse_code` | `VARCHAR(10)` | Required and unique |
| `warehouse_name` | `VARCHAR(100)` | Required |
| `city` | `VARCHAR(80)` | Required |
| `state` | `VARCHAR(80)` | Required |
| `capacity_units` | `INT UNSIGNED` | Required; must be greater than `0` |
| `occupied_units` | `INT UNSIGNED` | Required; default `0`; cannot exceed capacity |
| `manager_name` | `VARCHAR(100)` | Required |
| `temperature_controlled` | `BOOLEAN` | Required; default `FALSE` |
| `operational_status` | `VARCHAR(20)` | Required; default `ACTIVE`; allow `ACTIVE`, `MAINTENANCE`, or `CLOSED` |
| `created_at` | `TIMESTAMP` | Required; default current timestamp |

## Required CHECK Constraints

| Suggested constraint name | Exact condition | Invalid examples rejected |
|---|---|---|
| `chk_warehouses_capacity` | `capacity_units > 0` | Zero-capacity warehouse |
| `chk_warehouses_occupancy` | `occupied_units BETWEEN 0 AND capacity_units` | Negative occupancy or occupancy above capacity |
| `chk_warehouses_temperature_flag` | `temperature_controlled IN (0, 1)` | Boolean values other than `0` or `1` |
| `chk_warehouses_status` | `operational_status IN ('ACTIVE', 'MAINTENANCE', 'CLOSED')` | Any unsupported warehouse status |

## A. CREATE Exercises

1. **WHS-C1:** Create `warehouses` with all required keys, defaults, and checks.
2. **WHS-C2:** Add a table-level check ensuring `occupied_units` is not greater than `capacity_units`.
3. **WHS-C3:** Inspect the generated DDL and verify that the warehouse code is unique.

## B. INSERT Exercises

| Code | Name | City | State | Capacity | Occupied | Manager | Temperature controlled | Status |
|---|---|---|---|---:|---:|---|---|---|
| WH-BLR-01 | Bengaluru Central | Bengaluru | Karnataka | 10000 | 6400 | Ajay Nair | FALSE | ACTIVE |
| WH-HYD-02 | Hyderabad North | Hyderabad | Telangana | 8000 | 7900 | Sana Reddy | TRUE | ACTIVE |
| WH-MUM-03 | Mumbai Port Store | Mumbai | Maharashtra | 15000 | 9000 | Rohit Shah | FALSE | MAINTENANCE |
| WH-KOC-04 | Kochi Cold Hub | Kochi | Kerala | 6000 | 3200 | Meera Varma | TRUE | Use default |
| WH-OLD-05 | Training Closed Store | Pune | Maharashtra | 2000 | 0 | Test Manager | FALSE | CLOSED |

1. **WHS-I1:** Insert the Bengaluru warehouse.
2. **WHS-I2:** Insert the Hyderabad and Mumbai warehouses in one statement.
3. **WHS-I3:** Insert the remaining warehouses and allow the default status for `WH-KOC-04`.
4. **WHS-I4:** Attempt an insert with zero capacity.
5. **WHS-I5:** Attempt to insert occupancy greater than capacity and another warehouse using a duplicate code.

## C. UPDATE Exercises

1. **WHS-U1:** Add `500` occupied units to `WH-BLR-01`.
2. **WHS-U2:** Reduce `WH-HYD-02` occupancy by `1000` units.
3. **WHS-U3:** Change `WH-MUM-03` from `MAINTENANCE` to `ACTIVE`.
4. **WHS-U4:** Increase the capacity of every temperature-controlled warehouse by `1000` units.
5. **WHS-U5:** Attempt to set occupied units above capacity and confirm that the check prevents the update.

## D. DELETE Exercises

1. **WHS-D1:** Preview and delete the closed warehouse `WH-OLD-05`.
2. **WHS-D2:** Insert a temporary empty warehouse with code `WH-TMP-99` and delete it by code.
3. **WHS-D3:** Preview and delete active warehouses whose occupied quantity is less than half of their capacity.

---

# 4. Shipment Tracking Register

**Suggested table:** `shipments`

## Scenario

A parcel service needs one table for basic shipment, delivery, cost, priority, and status information.

## Column Requirements

| Column | Suggested type | Constraints and rules |
|---|---|---|
| `shipment_id` | `BIGINT UNSIGNED` | Primary key and auto-increment |
| `tracking_number` | `VARCHAR(20)` | Required and unique |
| `sender_name` | `VARCHAR(120)` | Required |
| `recipient_name` | `VARCHAR(120)` | Required |
| `destination_city` | `VARCHAR(80)` | Required |
| `package_weight_kg` | `DECIMAL(8,2)` | Required; greater than `0` and no more than `1000` |
| `shipping_cost` | `DECIMAL(10,2)` | Required; cannot be negative |
| `shipped_date` | `DATE` | Required |
| `expected_delivery_date` | `DATE` | Required; cannot be earlier than shipped date |
| `delivered_date` | `DATE` | Optional; if present, cannot be earlier than shipped date |
| `shipment_status` | `VARCHAR(20)` | Required; default `CREATED`; allow `CREATED`, `IN_TRANSIT`, `DELIVERED`, or `CANCELLED` |
| `priority` | `VARCHAR(10)` | Required; default `NORMAL`; allow `NORMAL` or `EXPRESS` |
| `created_at` | `TIMESTAMP` | Required; default current timestamp |

## Required CHECK Constraints

| Suggested constraint name | Exact condition | Invalid examples rejected |
|---|---|---|
| `chk_shipments_weight` | `package_weight_kg > 0 AND package_weight_kg <= 1000` | Zero, negative, or excessive weight |
| `chk_shipments_cost` | `shipping_cost >= 0` | Negative shipping cost |
| `chk_shipments_expected_date` | `expected_delivery_date >= shipped_date` | Expected delivery before shipment |
| `chk_shipments_delivered_date` | `delivered_date IS NULL OR delivered_date >= shipped_date` | A non-null delivery date before shipment |
| `chk_shipments_status` | `shipment_status IN ('CREATED', 'IN_TRANSIT', 'DELIVERED', 'CANCELLED')` | Any unsupported shipment status |
| `chk_shipments_priority` | `priority IN ('NORMAL', 'EXPRESS')` | Any unsupported priority |

## A. CREATE Exercises

1. **SHP-C1:** Create `shipments` with the specified keys, nullability, defaults, and checks.
2. **SHP-C2:** Add separate named checks for weight, cost, expected-delivery date, and delivered date.
3. **SHP-C3:** Verify that `delivered_date` accepts `NULL` while required dates do not.

## B. INSERT Exercises

| Tracking number | Sender | Recipient | Destination | Weight | Cost | Shipped | Expected | Delivered | Status | Priority |
|---|---|---|---|---:|---:|---|---|---|---|---|
| TRK26000001 | Asha Stores | Nitin Rao | Chennai | 2.50 | 180.00 | 2026-09-20 | 2026-09-24 | NULL | IN_TRANSIT | NORMAL |
| TRK26000002 | Nova Tech | Isha Sen | Bengaluru | 0.80 | 250.00 | 2026-09-21 | 2026-09-23 | 2026-09-23 | DELIVERED | EXPRESS |
| TRK26000003 | Book Nest | Omar Ali | Kochi | 4.20 | 220.00 | 2026-09-22 | 2026-09-27 | NULL | Use default | Use default |
| TRK26000004 | Green Foods | Meera Das | Hyderabad | 12.00 | 550.00 | 2026-09-22 | 2026-09-25 | NULL | IN_TRANSIT | EXPRESS |
| TRK26000005 | Test Sender | Test Receiver | Pune | 1.00 | 100.00 | 2026-09-23 | 2026-09-26 | NULL | CANCELLED | NORMAL |

1. **SHP-I1:** Insert the first shipment.
2. **SHP-I2:** Insert the delivered shipment and preserve its delivered date.
3. **SHP-I3:** Insert the third shipment while allowing both defaults to apply.
4. **SHP-I4:** Insert the remaining two shipments using one multi-row statement.
5. **SHP-I5:** Attempt inserts with zero weight, negative cost, an expected date before the shipped date, and a duplicate tracking number.

## C. UPDATE Exercises

1. **SHP-U1:** Change `TRK26000001` to `DELIVERED` and set its delivered date to `2026-09-24`.
2. **SHP-U2:** Change `TRK26000003` from its default status to `IN_TRANSIT`.
3. **SHP-U3:** Increase the cost of every express shipment by `50.00`.
4. **SHP-U4:** Postpone the expected delivery of `TRK26000004` to `2026-09-27`.
5. **SHP-U5:** Attempt to set a delivered date earlier than the shipped date.

## D. DELETE Exercises

1. **SHP-D1:** Preview and delete the cancelled shipment `TRK26000005`.
2. **SHP-D2:** Insert a temporary shipment with tracking number `TRK-TEMP-999` and delete it by tracking number.
3. **SHP-D3:** Preview and delete delivered express shipments weighing less than `1` kilogram.

---

# 5. Service Subscription Register

**Suggested table:** `subscriptions`

## Scenario

A digital service needs one independent table for subscriber, plan, billing, renewal, date, and status information.

## Column Requirements

| Column | Suggested type | Constraints and rules |
|---|---|---|
| `subscription_id` | `BIGINT UNSIGNED` | Primary key and auto-increment |
| `subscription_code` | `VARCHAR(16)` | Required and unique |
| `subscriber_name` | `VARCHAR(120)` | Required |
| `subscriber_email` | `VARCHAR(120)` | Required and unique |
| `plan_name` | `VARCHAR(60)` | Required |
| `billing_cycle` | `VARCHAR(20)` | Required; default `MONTHLY`; allow `MONTHLY`, `QUARTERLY`, or `YEARLY` |
| `amount` | `DECIMAL(10,2)` | Required; must be greater than `0` |
| `start_date` | `DATE` | Required |
| `end_date` | `DATE` | Optional; if present, cannot be earlier than start date |
| `auto_renew` | `BOOLEAN` | Required; default `TRUE` |
| `subscription_status` | `VARCHAR(20)` | Required; default `ACTIVE`; allow `ACTIVE`, `PAUSED`, `CANCELLED`, or `EXPIRED` |
| `created_at` | `TIMESTAMP` | Required; default current timestamp |

## Required CHECK Constraints

| Suggested constraint name | Exact condition | Invalid examples rejected |
|---|---|---|
| `chk_subscriptions_billing_cycle` | `billing_cycle IN ('MONTHLY', 'QUARTERLY', 'YEARLY')` | Any unsupported billing cycle |
| `chk_subscriptions_amount` | `amount > 0` | Zero or negative subscription amount |
| `chk_subscriptions_end_date` | `end_date IS NULL OR end_date >= start_date` | A non-null end date before the start date |
| `chk_subscriptions_auto_renew` | `auto_renew IN (0, 1)` | Boolean values other than `0` or `1` |
| `chk_subscriptions_status` | `subscription_status IN ('ACTIVE', 'PAUSED', 'CANCELLED', 'EXPIRED')` | Any unsupported subscription status |

## A. CREATE Exercises

1. **SUB-C1:** Create the table with a primary key, two unique constraints, defaults, and named checks.
2. **SUB-C2:** Ensure that amount is positive and end date is valid when supplied.
3. **SUB-C3:** Confirm that email and subscription code are independently unique.

## B. INSERT Exercises

| Code | Subscriber | Email | Plan | Cycle | Amount | Start date | End date | Auto-renew | Status |
|---|---|---|---|---|---:|---|---|---|---|
| SUB260001 | Aarav Nair | aarav.nair@example.test | Standard | MONTHLY | 499.00 | 2026-09-01 | NULL | TRUE | ACTIVE |
| SUB260002 | Diya Shah | diya.shah@example.test | Premium | YEARLY | 4999.00 | 2026-01-15 | 2027-01-14 | TRUE | ACTIVE |
| SUB260003 | Kabir Rao | kabir.rao@example.test | Basic | Use default | 199.00 | 2026-09-10 | NULL | Use default | Use default |
| SUB260004 | Meera Bose | meera.bose@example.test | Premium | QUARTERLY | 1399.00 | 2026-07-01 | 2026-09-30 | FALSE | PAUSED |
| SUB260005 | Test Subscriber | test.subscriber@example.test | Standard | MONTHLY | 499.00 | 2026-08-01 | 2026-08-31 | FALSE | CANCELLED |

1. **SUB-I1:** Insert the first two subscriptions.
2. **SUB-I2:** Insert `SUB260003` while allowing all specified defaults to apply.
3. **SUB-I3:** Insert the remaining two subscriptions.
4. **SUB-I4:** Attempt to insert a duplicate email and a duplicate subscription code.
5. **SUB-I5:** Attempt inserts having zero amount, an end date before the start date, and billing cycle `WEEKLY`.

## C. UPDATE Exercises

1. **SUB-U1:** Upgrade `SUB260001` to `Premium` and change its amount to `799.00`.
2. **SUB-U2:** Change `SUB260004` from `PAUSED` to `ACTIVE` and enable auto-renewal.
3. **SUB-U3:** Increase the amount of every monthly active subscription by `5%`.
4. **SUB-U4:** Cancel `SUB260003`, disable auto-renewal, and set an appropriate end date.
5. **SUB-U5:** Attempt to set an end date earlier than the start date.

## D. DELETE Exercises

1. **SUB-D1:** Preview and delete the test subscription `SUB260005`.
2. **SUB-D2:** Insert a temporary subscription with code `SUB-TEMP-999` and delete it by code.
3. **SUB-D3:** Preview and delete cancelled subscriptions for which auto-renew is false.

---

# 6. Event Schedule

**Suggested table:** `events`

## Scenario

An event organiser needs one table containing event schedule, venue, capacity, fee, format, and status information.

## Column Requirements

| Column | Suggested type | Constraints and rules |
|---|---|---|
| `event_id` | `INT UNSIGNED` | Primary key and auto-increment |
| `event_code` | `VARCHAR(15)` | Required and unique |
| `event_name` | `VARCHAR(150)` | Required |
| `venue` | `VARCHAR(150)` | Required |
| `city` | `VARCHAR(80)` | Required |
| `start_datetime` | `DATETIME` | Required |
| `end_datetime` | `DATETIME` | Required; must be later than start time |
| `capacity` | `INT UNSIGNED` | Required; must be greater than `0` |
| `registration_fee` | `DECIMAL(10,2)` | Required; default `0.00`; cannot be negative |
| `event_type` | `VARCHAR(20)` | Required; default `OFFLINE`; allow `ONLINE`, `OFFLINE`, or `HYBRID` |
| `event_status` | `VARCHAR(20)` | Required; default `PLANNED`; allow `PLANNED`, `OPEN`, `CLOSED`, `CANCELLED`, or `COMPLETED` |
| `created_at` | `TIMESTAMP` | Required; default current timestamp |

## Required CHECK Constraints

| Suggested constraint name | Exact condition | Invalid examples rejected |
|---|---|---|
| `chk_events_datetime_order` | `end_datetime > start_datetime` | End time equal to or before start time |
| `chk_events_capacity` | `capacity > 0` | Zero or negative capacity |
| `chk_events_fee` | `registration_fee >= 0` | Negative registration fee |
| `chk_events_type` | `event_type IN ('ONLINE', 'OFFLINE', 'HYBRID')` | Any unsupported event type |
| `chk_events_status` | `event_status IN ('PLANNED', 'OPEN', 'CLOSED', 'CANCELLED', 'COMPLETED')` | Any unsupported event status |

## A. CREATE Exercises

1. **EVT-C1:** Create the event table with all specified constraints.
2. **EVT-C2:** Add named checks for date order, capacity, fee, event type, and event status.
3. **EVT-C3:** Verify all defaults through `SHOW CREATE TABLE`.

## B. INSERT Exercises

| Code | Event name | Venue | City | Start | End | Capacity | Fee | Type | Status |
|---|---|---|---|---|---|---:|---:|---|---|
| EVT-JAVA-01 | Java Developer Meetup | Tech Hall | Bengaluru | 2026-10-10 09:30:00 | 2026-10-10 16:30:00 | 250 | 500.00 | OFFLINE | OPEN |
| EVT-CLOUD-02 | Cloud Fundamentals Webinar | Online Platform | Online | 2026-10-12 18:00:00 | 2026-10-12 20:00:00 | 1000 | 0.00 | ONLINE | OPEN |
| EVT-DATA-03 | Data Engineering Summit | Convention Centre | Hyderabad | 2026-11-05 09:00:00 | 2026-11-06 17:00:00 | 600 | 2500.00 | HYBRID | PLANNED |
| EVT-UI-04 | UI Design Workshop | Creative Hub | Chennai | 2026-10-20 10:00:00 | 2026-10-20 15:00:00 | 80 | 1200.00 | Use default | Use default |
| EVT-OLD-05 | Cancelled Training Event | Test Venue | Pune | 2026-10-01 10:00:00 | 2026-10-01 12:00:00 | 30 | 100.00 | OFFLINE | CANCELLED |

1. **EVT-I1:** Insert the first event.
2. **EVT-I2:** Insert the webinar and summit using one multi-row statement.
3. **EVT-I3:** Insert the workshop while allowing both defaults to apply.
4. **EVT-I4:** Insert the cancelled event.
5. **EVT-I5:** Attempt inserts having zero capacity, negative fee, end time before start time, and event type `RECORDED`.

## C. UPDATE Exercises

1. **EVT-U1:** Increase the summit capacity to `750`.
2. **EVT-U2:** Change `EVT-UI-04` status from its default to `OPEN`.
3. **EVT-U3:** Increase the registration fee of every paid offline event by `10%`.
4. **EVT-U4:** Reschedule both timestamps of `EVT-CLOUD-02` one day later.
5. **EVT-U5:** Attempt to set an event end time equal to its start time.

## D. DELETE Exercises

1. **EVT-D1:** Preview and delete `EVT-OLD-05`.
2. **EVT-D2:** Insert an event with code `EVT-TEMP-99` and delete it by code.
3. **EVT-D3:** Preview and delete free online events whose status is `OPEN`.

---

# 7. Organisation Device Register

**Suggested table:** `devices`

## Scenario

An organisation needs one table for its technology assets, purchase information, warranty, assignment, and status.

## Column Requirements

| Column | Suggested type | Constraints and rules |
|---|---|---|
| `device_id` | `BIGINT UNSIGNED` | Primary key and auto-increment |
| `asset_tag` | `VARCHAR(20)` | Required and unique |
| `serial_number` | `VARCHAR(50)` | Required and unique |
| `device_name` | `VARCHAR(120)` | Required |
| `device_type` | `VARCHAR(20)` | Required; allow `LAPTOP`, `DESKTOP`, `TABLET`, `PHONE`, or `ROUTER` |
| `manufacturer` | `VARCHAR(80)` | Required |
| `model` | `VARCHAR(100)` | Required |
| `purchase_date` | `DATE` | Required |
| `purchase_price` | `DECIMAL(12,2)` | Required; cannot be negative |
| `warranty_expiry` | `DATE` | Optional; if present, cannot be earlier than purchase date |
| `assigned_to` | `VARCHAR(120)` | Optional |
| `device_status` | `VARCHAR(20)` | Required; default `AVAILABLE`; allow `AVAILABLE`, `ASSIGNED`, `REPAIR`, `LOST`, or `RETIRED` |
| `created_at` | `TIMESTAMP` | Required; default current timestamp |

## Required CHECK Constraints

| Suggested constraint name | Exact condition | Invalid examples rejected |
|---|---|---|
| `chk_devices_type` | `device_type IN ('LAPTOP', 'DESKTOP', 'TABLET', 'PHONE', 'ROUTER')` | Any unsupported device type |
| `chk_devices_price` | `purchase_price >= 0` | Negative purchase price |
| `chk_devices_warranty_date` | `warranty_expiry IS NULL OR warranty_expiry >= purchase_date` | A non-null warranty date before purchase |
| `chk_devices_status` | `device_status IN ('AVAILABLE', 'ASSIGNED', 'REPAIR', 'LOST', 'RETIRED')` | Any unsupported device status |

## A. CREATE Exercises

1. **DEV-C1:** Create `devices` with a primary key, two unique constraints, a default status, and named checks.
2. **DEV-C2:** Ensure that price and warranty dates satisfy the stated rules.
3. **DEV-C3:** Confirm that assignment and warranty columns accept `NULL`.

## B. INSERT Exercises

| Asset tag | Serial number | Name | Type | Manufacturer | Model | Purchased | Price | Warranty | Assigned to | Status |
|---|---|---|---|---|---|---|---:|---|---|---|
| AST-LAP-001 | SN-LAP-260001 | Developer Laptop | LAPTOP | Lenovo | ThinkPad E14 | 2026-01-15 | 78000.00 | 2029-01-14 | Kavya Menon | ASSIGNED |
| AST-DES-002 | SN-DES-260002 | Lab Desktop | DESKTOP | Dell | OptiPlex 7010 | 2025-08-10 | 62000.00 | 2028-08-09 | NULL | AVAILABLE |
| AST-TAB-003 | SN-TAB-260003 | Demo Tablet | TABLET | Samsung | Galaxy Tab S9 | 2026-03-05 | 68000.00 | 2028-03-04 | NULL | Use default |
| AST-RTR-004 | SN-RTR-260004 | Office Router | ROUTER | Cisco | RV340 | 2024-06-20 | 28000.00 | NULL | Network Team | REPAIR |
| AST-OLD-005 | SN-OLD-260005 | Retired Phone | PHONE | Nokia | Test Model | 2020-01-10 | 8000.00 | 2021-01-09 | NULL | RETIRED |

1. **DEV-I1:** Insert the laptop.
2. **DEV-I2:** Insert the desktop and tablet, allowing the tablet's default status to apply.
3. **DEV-I3:** Insert the remaining two devices.
4. **DEV-I4:** Attempt to insert duplicate asset and serial values.
5. **DEV-I5:** Attempt inserts with a negative price, unsupported type `PRINTER`, and warranty expiry before purchase date.

## C. UPDATE Exercises

1. **DEV-U1:** Assign `AST-DES-002` to `Rohan Das` and change its status to `ASSIGNED`.
2. **DEV-U2:** Change `AST-RTR-004` from `REPAIR` to `AVAILABLE` and clear its assignee.
3. **DEV-U3:** Extend the laptop warranty to `2030-01-14`.
4. **DEV-U4:** Increase the recorded purchase price of every tablet by `3%` for a valuation exercise.
5. **DEV-U5:** Attempt to change a warranty date to a value before purchase date.

## D. DELETE Exercises

1. **DEV-D1:** Preview and delete the retired test device `AST-OLD-005`.
2. **DEV-D2:** Insert a temporary device tagged `AST-TEMP-999` and delete it by asset tag.
3. **DEV-D3:** Preview and delete available devices that have no assignee but still have a warranty-expiry date.

---

# 8. Restaurant Directory

**Suggested table:** `restaurants`

## Scenario

A dining directory needs one table containing restaurant identity, contact, capacity, price, rating, food preference, and operating status.

## Column Requirements

| Column | Suggested type | Constraints and rules |
|---|---|---|
| `restaurant_id` | `INT UNSIGNED` | Primary key and auto-increment |
| `restaurant_code` | `VARCHAR(12)` | Required and unique |
| `restaurant_name` | `VARCHAR(120)` | Required |
| `cuisine_type` | `VARCHAR(80)` | Required |
| `city` | `VARCHAR(80)` | Required |
| `phone` | `VARCHAR(15)` | Required and unique |
| `seating_capacity` | `INT UNSIGNED` | Required; must be greater than `0` |
| `average_cost_for_two` | `DECIMAL(10,2)` | Required; must be greater than `0` |
| `rating` | `DECIMAL(2,1)` | Required; default `0.0`; range `0.0` through `5.0` |
| `vegetarian_only` | `BOOLEAN` | Required; default `FALSE` |
| `opening_status` | `VARCHAR(25)` | Required; default `OPEN`; allow `OPEN`, `TEMPORARILY_CLOSED`, or `PERMANENTLY_CLOSED` |
| `opened_date` | `DATE` | Required |
| `created_at` | `TIMESTAMP` | Required; default current timestamp |

## Required CHECK Constraints

| Suggested constraint name | Exact condition | Invalid examples rejected |
|---|---|---|
| `chk_restaurants_capacity` | `seating_capacity > 0` | Zero or negative seating capacity |
| `chk_restaurants_cost` | `average_cost_for_two > 0` | Zero or negative average cost |
| `chk_restaurants_rating` | `rating BETWEEN 0.0 AND 5.0` | Rating below `0.0` or above `5.0` |
| `chk_restaurants_vegetarian_flag` | `vegetarian_only IN (0, 1)` | Boolean values other than `0` or `1` |
| `chk_restaurants_status` | `opening_status IN ('OPEN', 'TEMPORARILY_CLOSED', 'PERMANENTLY_CLOSED')` | Any unsupported opening status |

## A. CREATE Exercises

1. **RES-C1:** Create the restaurant table with all specified constraints.
2. **RES-C2:** Use separate unique constraints for restaurant code and phone.
3. **RES-C3:** Add and verify checks for capacity, cost, rating, and opening status.

## B. INSERT Exercises

| Code | Name | Cuisine | City | Phone | Seats | Cost for two | Rating | Vegetarian only | Status | Opened date |
|---|---|---|---|---|---:|---:|---:|---|---|---|
| RES-BLR-001 | Spice Courtyard | Indian | Bengaluru | 9876601001 | 80 | 1200.00 | 4.4 | FALSE | OPEN | 2018-06-15 |
| RES-CHE-002 | Green Banana Leaf | South Indian | Chennai | 9876601002 | 60 | 700.00 | 4.6 | TRUE | OPEN | 2020-02-10 |
| RES-MUM-003 | Coastal Plate | Seafood | Mumbai | 9876601003 | 100 | 1800.00 | 4.2 | FALSE | OPEN | 2017-09-21 |
| RES-HYD-004 | Urban Bowl | Fusion | Hyderabad | 9876601004 | 45 | 950.00 | Use default | Use default | Use default | 2026-01-12 |
| RES-OLD-005 | Training Cafe | Cafe | Pune | 9876601005 | 20 | 400.00 | 2.0 | FALSE | PERMANENTLY_CLOSED | 2015-05-01 |

1. **RES-I1:** Insert the first restaurant.
2. **RES-I2:** Insert the next two restaurants using one multi-row statement.
3. **RES-I3:** Insert `RES-HYD-004` while allowing all specified defaults to apply.
4. **RES-I4:** Insert the permanently closed training restaurant.
5. **RES-I5:** Attempt inserts with a duplicate phone, zero seating, zero cost, rating `5.5`, and status `RENOVATING`.

## C. UPDATE Exercises

1. **RES-U1:** Change the rating of `RES-BLR-001` to `4.5`.
2. **RES-U2:** Increase average cost by `10%` for restaurants in Mumbai.
3. **RES-U3:** Change `RES-HYD-004` rating to `4.0` and mark it vegetarian-only.
4. **RES-U4:** Increase seating capacity by `10` for every open restaurant with fewer than `60` seats.
5. **RES-U5:** Attempt to set a rating above `5.0`.

## D. DELETE Exercises

1. **RES-D1:** Preview and delete `RES-OLD-005`.
2. **RES-D2:** Insert a restaurant with code `RES-TEMP-999` and delete it by code.
3. **RES-D3:** Preview and delete open restaurants having a rating below `4.1`.

---

# 9. Insurance Policy Register

**Suggested table:** `insurance_policies`

## Scenario

A fictional training system needs one simplified table for policyholder, coverage, premium, term, payment, nominee, and status information.

## Column Requirements

| Column | Suggested type | Constraints and rules |
|---|---|---|
| `policy_id` | `BIGINT UNSIGNED` | Primary key and auto-increment |
| `policy_number` | `CHAR(14)` | Required and unique |
| `policyholder_name` | `VARCHAR(120)` | Required |
| `policy_type` | `VARCHAR(20)` | Required; allow `HEALTH`, `VEHICLE`, `LIFE`, `TRAVEL`, or `HOME` |
| `coverage_amount` | `DECIMAL(15,2)` | Required; must be greater than `0` |
| `premium_amount` | `DECIMAL(12,2)` | Required; greater than `0` and not greater than coverage amount |
| `start_date` | `DATE` | Required |
| `end_date` | `DATE` | Required; must be later than start date |
| `payment_frequency` | `VARCHAR(20)` | Required; default `ANNUAL`; allow `MONTHLY`, `QUARTERLY`, `HALF_YEARLY`, or `ANNUAL` |
| `nominee_name` | `VARCHAR(120)` | Optional |
| `policy_status` | `VARCHAR(20)` | Required; default `ACTIVE`; allow `ACTIVE`, `LAPSED`, `CANCELLED`, or `EXPIRED` |
| `created_at` | `TIMESTAMP` | Required; default current timestamp |

## Required CHECK Constraints

| Suggested constraint name | Exact condition | Invalid examples rejected |
|---|---|---|
| `chk_policies_type` | `policy_type IN ('HEALTH', 'VEHICLE', 'LIFE', 'TRAVEL', 'HOME')` | Any unsupported policy type |
| `chk_policies_coverage` | `coverage_amount > 0` | Zero or negative coverage |
| `chk_policies_premium_positive` | `premium_amount > 0` | Zero or negative premium |
| `chk_policies_premium_limit` | `premium_amount <= coverage_amount` | Premium greater than coverage |
| `chk_policies_date_order` | `end_date > start_date` | End date equal to or before start date |
| `chk_policies_payment_frequency` | `payment_frequency IN ('MONTHLY', 'QUARTERLY', 'HALF_YEARLY', 'ANNUAL')` | Any unsupported payment frequency |
| `chk_policies_status` | `policy_status IN ('ACTIVE', 'LAPSED', 'CANCELLED', 'EXPIRED')` | Any unsupported policy status |

## A. CREATE Exercises

1. **POL-C1:** Create `insurance_policies` with all required constraints and defaults.
2. **POL-C2:** Add named checks for policy type, positive amounts, premium-versus-coverage, date order, payment frequency, and status.
3. **POL-C3:** Verify that nominee is optional while all policy and term fields are required.

## B. INSERT Exercises

| Policy number | Holder | Type | Coverage | Premium | Start | End | Frequency | Nominee | Status |
|---|---|---|---:|---:|---|---|---|---|---|
| POL00000000001 | Aditi Rao | HEALTH | 1000000.00 | 18000.00 | 2026-01-01 | 2026-12-31 | ANNUAL | Rohan Rao | ACTIVE |
| POL00000000002 | Dev Kapoor | VEHICLE | 800000.00 | 22000.00 | 2026-04-15 | 2027-04-14 | QUARTERLY | NULL | ACTIVE |
| POL00000000003 | Meera Nair | LIFE | 5000000.00 | 36000.00 | 2026-06-01 | 2046-05-31 | Use default | Arun Nair | Use default |
| POL00000000004 | Kabir Shah | TRAVEL | 500000.00 | 4500.00 | 2026-10-01 | 2026-10-31 | MONTHLY | NULL | ACTIVE |
| POL00000000005 | Test Policyholder | HOME | 2000000.00 | 15000.00 | 2025-01-01 | 2025-12-31 | ANNUAL | NULL | CANCELLED |

1. **POL-I1:** Insert the health and vehicle policies.
2. **POL-I2:** Insert the life policy and allow both defaults to apply.
3. **POL-I3:** Insert the remaining two policies.
4. **POL-I4:** Attempt to insert a duplicate policy number.
5. **POL-I5:** Attempt inserts having zero coverage, zero premium, premium greater than coverage, an invalid date range, and policy type `BUSINESS`.

## C. UPDATE Exercises

1. **POL-U1:** Increase the coverage of `POL00000000001` to `1250000.00`.
2. **POL-U2:** Add nominee `Sara Kapoor` to `POL00000000002`.
3. **POL-U3:** Increase premiums of active vehicle policies by `5%` without violating the coverage check.
4. **POL-U4:** Change `POL00000000004` payment frequency to `ANNUAL`.
5. **POL-U5:** Attempt to set a premium greater than its coverage amount.

## D. DELETE Exercises

1. **POL-D1:** Preview and delete fictional cancelled policy `POL00000000005`.
2. **POL-D2:** Insert a temporary fictional policy numbered `POL99999999999` and delete it by policy number.
3. **POL-D3:** Preview and delete travel policies having no nominee and a premium below `5000.00`.

---

# 10. Job Posting Board

**Suggested table:** `job_postings`

## Scenario

A recruitment portal needs one independent table for job descriptions, location, employment type, salary range, vacancies, dates, and status.

## Column Requirements

| Column | Suggested type | Constraints and rules |
|---|---|---|
| `job_id` | `BIGINT UNSIGNED` | Primary key and auto-increment |
| `job_code` | `VARCHAR(15)` | Required and unique |
| `job_title` | `VARCHAR(150)` | Required |
| `department` | `VARCHAR(100)` | Required |
| `location` | `VARCHAR(100)` | Required |
| `employment_type` | `VARCHAR(20)` | Required; default `FULL_TIME`; allow `FULL_TIME`, `PART_TIME`, `CONTRACT`, or `INTERNSHIP` |
| `minimum_salary` | `DECIMAL(12,2)` | Required; cannot be negative |
| `maximum_salary` | `DECIMAL(12,2)` | Required; cannot be lower than minimum salary |
| `vacancies` | `INT UNSIGNED` | Required; default `1`; must be greater than `0` |
| `posted_date` | `DATE` | Required |
| `closing_date` | `DATE` | Required; cannot be earlier than posted date |
| `job_status` | `VARCHAR(20)` | Required; default `OPEN`; allow `DRAFT`, `OPEN`, `CLOSED`, or `CANCELLED` |
| `created_at` | `TIMESTAMP` | Required; default current timestamp |

## Required CHECK Constraints

| Suggested constraint name | Exact condition | Invalid examples rejected |
|---|---|---|
| `chk_job_postings_employment_type` | `employment_type IN ('FULL_TIME', 'PART_TIME', 'CONTRACT', 'INTERNSHIP')` | Any unsupported employment type |
| `chk_job_postings_minimum_salary` | `minimum_salary >= 0` | Negative minimum salary |
| `chk_job_postings_salary_range` | `maximum_salary >= minimum_salary` | Maximum salary below minimum salary |
| `chk_job_postings_vacancies` | `vacancies > 0` | Zero vacancies |
| `chk_job_postings_closing_date` | `closing_date >= posted_date` | Closing date before posting date |
| `chk_job_postings_status` | `job_status IN ('DRAFT', 'OPEN', 'CLOSED', 'CANCELLED')` | Any unsupported job status |

## A. CREATE Exercises

1. **JOB-C1:** Create `job_postings` with all specified constraints and defaults.
2. **JOB-C2:** Add named checks for employment type, salary values, vacancies, closing date, and job status.
3. **JOB-C3:** Verify that job code is unique and every descriptive field is required.

## B. INSERT Exercises

| Job code | Title | Department | Location | Type | Minimum salary | Maximum salary | Vacancies | Posted | Closing | Status |
|---|---|---|---|---|---:|---:|---:|---|---|---|
| JOB-JAVA-001 | Java Developer | Engineering | Bengaluru | FULL_TIME | 700000.00 | 1200000.00 | 3 | 2026-09-20 | 2026-10-20 | OPEN |
| JOB-QA-002 | QA Automation Engineer | Quality Assurance | Hyderabad | CONTRACT | 600000.00 | 900000.00 | 2 | 2026-09-21 | 2026-10-15 | OPEN |
| JOB-INT-003 | Software Intern | Engineering | Chennai | INTERNSHIP | 180000.00 | 240000.00 | Use default | 2026-09-22 | 2026-10-10 | Use default |
| JOB-HR-004 | HR Coordinator | Human Resources | Mumbai | PART_TIME | 300000.00 | 450000.00 | 1 | 2026-09-23 | 2026-10-18 | DRAFT |
| JOB-OLD-005 | Cancelled Test Role | Training | Pune | FULL_TIME | 250000.00 | 350000.00 | 1 | 2026-09-01 | 2026-09-15 | CANCELLED |

1. **JOB-I1:** Insert the Java and QA positions.
2. **JOB-I2:** Insert the internship with vacancies omitted so that its default is used. Explicitly supply `INTERNSHIP` because the employment-type default is `FULL_TIME`.
3. **JOB-I3:** Insert the remaining positions.
4. **JOB-I4:** Attempt to insert a duplicate job code.
5. **JOB-I5:** Attempt inserts with negative minimum salary, maximum salary below minimum, zero vacancies, closing date before posted date, and employment type `TEMPORARY`.

## C. UPDATE Exercises

1. **JOB-U1:** Increase the maximum salary of `JOB-JAVA-001` to `1300000.00`.
2. **JOB-U2:** Increase vacancies for `JOB-QA-002` from `2` to `4`.
3. **JOB-U3:** Change `JOB-HR-004` from `DRAFT` to `OPEN`.
4. **JOB-U4:** Increase both salary limits of every open engineering job by `8%`.
5. **JOB-U5:** Attempt to make maximum salary lower than minimum salary.

## D. DELETE Exercises

1. **JOB-D1:** Preview and delete `JOB-OLD-005`.
2. **JOB-D2:** Insert a temporary posting with code `JOB-TEMP-999` and delete it by code.
3. **JOB-D3:** Preview and delete internship postings whose maximum salary is at most `250000.00`.

---

# Final Constraint Challenges

Complete these after finishing the 10 use cases:

1. Identify the primary-key column in every table.
2. List every business column protected by a unique constraint.
3. Find one table with two independent unique constraints.
4. Demonstrate a `NOT NULL` failure in three different tables.
5. Demonstrate a default value in every table by omitting the relevant column from an insert.
6. Demonstrate one numeric check failure in every applicable table.
7. Demonstrate one date-order check failure in shipments, subscriptions, events, devices, policies, and job postings.
8. Perform one single-row update using a unique business key in every table.
9. Perform one conditional multi-row update in at least five tables.
10. Insert and then permanently delete one temporary row from every table.

# Submission Checklist

- All 10 tables were created successfully.
- Every table has an auto-increment primary key.
- Every table has at least one unique business identifier.
- Required columns use `NOT NULL`.
- Default values were defined and tested.
- Check constraints have meaningful names.
- Valid single-row inserts were completed.
- Valid multi-row inserts were completed.
- Invalid inserts were attempted and their errors recorded.
- Single-row updates use unique identifiers.
- Multi-row updates use carefully reviewed conditions.
- Every delete was previewed using `SELECT`.
- Every delete contains a precise `WHERE` clause.
