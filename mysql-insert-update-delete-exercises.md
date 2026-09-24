# MySQL INSERT, UPDATE, and DELETE Practice Pack

## Purpose

This exercise pack reuses the 10 standalone tables from the earlier table-design exercises:

1. `students`
2. `products`
3. `customers`
4. `books`
5. `patients`
6. `bank_accounts`
7. `vehicles`
8. `hotel_rooms`
9. `movies`
10. `support_tickets`

The exercises focus on writing safe and correct `INSERT`, `UPDATE`, and `DELETE` statements. SQL solutions are intentionally omitted.

## Prerequisites

- Create all 10 tables using the earlier DDL exercise pack.
- Use MySQL 8.x or a compatible MySQL version.
- Execute these exercises only in a practice database.
- Use fictional data only, especially for patient and bank-account exercises.

## Safe DML Workflow

Follow this process before every `UPDATE` or `DELETE` exercise:

1. Write a `SELECT` statement using the proposed `WHERE` condition.
2. Confirm that only the intended rows are returned.
3. Start a transaction when testing a risky operation.
4. Execute the `UPDATE` or `DELETE` statement.
5. Check the affected-row count.
6. Verify the result using `SELECT`.
7. Use `COMMIT` only when the result is correct; otherwise use `ROLLBACK`.

> Never execute an `UPDATE` or `DELETE` without a `WHERE` clause unless the exercise explicitly requires all rows to be changed or removed.

## General Instructions

- Complete each use case in the order shown: Insert, Update, and then Delete.
- The exercises inside each use case are cumulative.
- Always provide an explicit column list in an `INSERT` statement.
- Do not manually insert auto-generated primary-key values.
- Allow timestamp defaults to operate unless an exercise says otherwise.
- Write SQL `NULL` without quotation marks.
- Use unique business identifiers such as admission number, SKU, ISBN, or ticket number in single-row `WHERE` conditions.
- Before repeating a section, remove its earlier practice rows or recreate the table.

---

# 1. Student DML Exercises

**Table:** `students`

## A. INSERT Exercises

Use the following data. Allow `student_id`, `created_at`, and `updated_at` to be generated automatically.

| Admission number | First name | Last name | Email | Phone | Date of birth | Programme | Admission date | CGPA | Status |
|---|---|---|---|---|---|---|---|---:|---|
| STU26C001 | Ananya | Rao | ananya.rao@example.test | 9876501001 | 2007-04-18 | BSc Computer Science | 2026-07-01 | 8.40 | ACTIVE |
| STU26C002 | Vivaan | Sharma | vivaan.sharma@example.test | NULL | 2006-12-09 | BCom | 2026-07-01 | 7.75 | ACTIVE |
| STU26C003 | Diya | Nair | diya.nair@example.test | 9876501003 | 2007-02-25 | BA Economics | 2026-07-02 | 9.10 | ACTIVE |
| STU25C004 | Kabir | Singh | kabir.singh@example.test | 9876501004 | 2006-08-14 | BSc Mathematics | 2025-07-01 | 6.85 | SUSPENDED |
| STU24C005 | Tara | Bose | tara.bose@example.test | 9876501005 | 2005-09-30 | BA History | 2024-07-01 | 5.90 | DROPPED |

1. **STU-I1:** Insert `STU26C001` using a single-row `INSERT`.
2. **STU-I2:** Insert `STU26C002` and preserve its `NULL` phone number.
3. **STU-I3:** Insert the remaining three students using one multi-row `INSERT`.
4. **STU-I4:** Attempt an insert using an existing email address. Record the MySQL error and explain which constraint rejected it.
5. **STU-I5:** Attempt an insert with a CGPA of `10.50` and another with status `TRANSFERRED`. Neither row should be stored.

## B. UPDATE Exercises

1. **STU-U1:** Change the CGPA of `STU26C001` to `8.65`.
2. **STU-U2:** Increase the CGPA of every active `BSc Computer Science` student by `0.20`. Ensure that the result cannot exceed `10.00`.
3. **STU-U3:** Change `STU25C004` from `SUSPENDED` to `ACTIVE`.
4. **STU-U4:** Rename the programme `BCom` to `BCom Finance` for matching students.
5. **STU-U5:** Try updating `STU26C003` to an email already used by another student. Confirm that the update fails.

## C. DELETE Exercises

1. **STU-D1:** Preview and permanently delete students whose status is `DROPPED`. Expect one row from the supplied data.
2. **STU-D2:** Insert a temporary student with admission number `STU-TEMP-001`, verify it, and delete only that row.
3. **STU-D3:** Start a transaction, delete all students in `BA Economics`, verify the temporary result, and then roll back the deletion.

---

# 2. Product DML Exercises

**Table:** `products`

## A. INSERT Exercises

Allow the primary key and timestamp columns to be generated automatically.

| SKU | Product name | Category | Brand | Unit price | Stock | Reorder level | Manufacture date | Expiry date | Status |
|---|---|---|---|---:|---:|---:|---|---|---|
| SKU-CBL-001 | USB-C Cable | Accessories | TechLine | 399.00 | 50 | 10 | NULL | NULL | ACTIVE |
| SKU-KBD-002 | Wireless Keyboard | Accessories | KeyPro | 1499.00 | 8 | 5 | 2026-01-15 | NULL | ACTIVE |
| SKU-JCE-003 | Orange Juice | Beverages | FreshDrop | 120.00 | 0 | 20 | 2026-09-01 | 2026-12-01 | OUT_OF_STOCK |
| SKU-NTB-004 | A5 Notebook | Stationery | PaperNest | 75.00 | 120 | 25 | NULL | NULL | ACTIVE |
| SKU-OLD-005 | Legacy Adapter | Accessories | WireMax | 299.00 | 0 | 0 | NULL | NULL | DISCONTINUED |

1. **PRD-I1:** Insert the cable using a single-row statement.
2. **PRD-I2:** Insert the keyboard and juice using one multi-row statement.
3. **PRD-I3:** Insert the notebook and discontinued adapter using one multi-row statement.
4. **PRD-I4:** Attempt to insert a product having a negative price.
5. **PRD-I5:** Attempt to insert a product whose expiry date is earlier than its manufacture date.
6. **PRD-I6:** Attempt to insert another product using `SKU-CBL-001`.

## B. UPDATE Exercises

1. **PRD-U1:** Add `60` units to the juice stock and change its status to `ACTIVE` in the same statement.
2. **PRD-U2:** Increase the price of every product in `Accessories` by `5%`. Round the new price to two decimal places.
3. **PRD-U3:** Set the notebook brand to `NULL` to practise updating an optional column.
4. **PRD-U4:** Set the reorder level to `15` for active products having fewer than `10` units in stock.
5. **PRD-U5:** Attempt to set a product's stock quantity to `-1`. Confirm that the constraint prevents the change.

## C. DELETE Exercises

1. **PRD-D1:** Preview and delete the product identified by `SKU-OLD-005`.
2. **PRD-D2:** Insert a temporary product with SKU `SKU-TEMP-999` and then delete it using its SKU.
3. **PRD-D3:** Start a transaction, delete every product in `Stationery`, check the temporary result, and roll back.

---

# 3. Customer DML Exercises

**Table:** `customers`

## A. INSERT Exercises

Allow `customer_id` and `registered_at` to be generated automatically.

| Customer code | First name | Last name | Email | Phone | Date of birth | City | State | Postal code | Type | Credit limit | Active |
|---|---|---|---|---|---|---|---|---|---|---:|---|
| CUST26001 | Ananya | Iyer | ananya.iyer@example.test | 9876502001 | 1995-04-11 | Bengaluru | Karnataka | 560001 | PREMIUM | 75000.00 | TRUE |
| CUST26002 | Rohan | Das | rohan.das@example.test | NULL | NULL | Kolkata | West Bengal | 700001 | REGULAR | 0.00 | TRUE |
| CUST26003 | Meera | Shah | meera.shah@example.test | 9876502003 | 1992-08-24 | Mumbai | Maharashtra | 400001 | CORPORATE | 250000.00 | TRUE |
| CUST26004 | Arjun | Reddy | arjun.reddy@example.test | 9876502004 | 1988-01-19 | Hyderabad | Telangana | 500001 | PREMIUM | 100000.00 | TRUE |
| CUST26005 | Nisha | Menon | nisha.menon@example.test | NULL | NULL | Kochi | Kerala | 682001 | REGULAR | 0.00 | FALSE |

1. **CUS-I1:** Insert the first customer using a single-row statement.
2. **CUS-I2:** Insert the second customer with both optional values set to `NULL`.
3. **CUS-I3:** Insert the remaining three customers using one multi-row statement.
4. **CUS-I4:** Attempt to insert a customer using a duplicate email address.
5. **CUS-I5:** Attempt inserts using a negative credit limit and customer type `GOLD`.

## B. UPDATE Exercises

1. **CUS-U1:** Increase the credit limit of every active premium customer by `10%`.
2. **CUS-U2:** Add phone number `9876502002` to customer `CUST26002`.
3. **CUS-U3:** Change the city of `CUST26004` to `Secunderabad` and its postal code to `500003`.
4. **CUS-U4:** Change the credit limit of `CUST26003` to `275000.00`.
5. **CUS-U5:** Attempt to update `CUST26002` with another customer's phone number and confirm the unique constraint.

## C. DELETE Exercises

1. **CUS-D1:** Preview and delete inactive customers. Expect `CUST26005` to match.
2. **CUS-D2:** Insert a temporary customer with code `CUST-TEMP-01` and delete only that row.
3. **CUS-D3:** Start a transaction, delete all `REGULAR` customers, verify the temporary result, and roll back.

---

# 4. Book DML Exercises

**Table:** `books`

## A. INSERT Exercises

Allow `book_id` and `added_at` to be generated automatically.

| ISBN | Title | Author | Genre | Publisher | Year | Pages | Format | Price | Copies | Language |
|---|---|---|---|---|---:|---:|---|---:|---:|---|
| 9780134685991 | Effective Java | Joshua Bloch | Programming | Addison-Wesley | 2018 | 416 | HARDCOVER | 4500.00 | 6 | English |
| 9780132350884 | Clean Code | Robert C. Martin | Programming | Prentice Hall | 2008 | 464 | PAPERBACK | 3200.00 | 12 | English |
| 9780262046305 | Introduction to Algorithms | Thomas H. Cormen | Computer Science | MIT Press | 2022 | 1312 | HARDCOVER | 6500.00 | 4 | English |
| 9780000000001 | The Monsoon Trail | Kavya Sen | Fiction | NULL | 2025 | 288 | PAPERBACK | 499.00 | 20 | English |
| 9780000000002 | Data Stories for Beginners | Asha Rao | Education | Learning House | 2026 | 210 | EBOOK | 299.00 | 0 | English |

1. **BOK-I1:** Insert the first book using a single-row statement.
2. **BOK-I2:** Insert the next two books using one multi-row statement.
3. **BOK-I3:** Insert the final two books, including the `NULL` publisher.
4. **BOK-I4:** Attempt an insert with a publication year of `999`.
5. **BOK-I5:** Attempt inserts having zero pages, a negative price, and format `AUDIOBOOK`.
6. **BOK-I6:** Attempt to insert a duplicate ISBN.

## B. UPDATE Exercises

1. **BOK-U1:** Add `10` copies to the available quantity of `Clean Code`.
2. **BOK-U2:** Reduce the price of every `EBOOK` by `10%`.
3. **BOK-U3:** Set the publisher of `The Monsoon Trail` to `Riverleaf Press`.
4. **BOK-U4:** Change the available copies of `Data Stories for Beginners` from `0` to `15`.
5. **BOK-U5:** Attempt to set the page count of a book to `0` and confirm that the update fails.

## C. DELETE Exercises

1. **BOK-D1:** Preview and delete the fictional book identified by ISBN `9780000000001`.
2. **BOK-D2:** Insert a temporary book with ISBN `9780000000999` and delete it by ISBN.
3. **BOK-D3:** Start a transaction, delete every `EBOOK`, inspect the result, and roll back.

---

# 5. Patient DML Exercises

**Table:** `patients`

## A. INSERT Exercises

All names and contact details below are fictional. Allow the primary key and registration timestamp to be generated automatically.

| Patient number | First name | Last name | Date of birth | Biological sex | Blood group | Phone | Email | Emergency contact | Emergency phone | Allergies | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|
| PT26001 | Aarya | Kapoor | 1998-05-12 | FEMALE | A+ | 9876503001 | aarya.kapoor@example.test | Rohan Kapoor | 9876513001 | Penicillin | ACTIVE |
| PT26002 | Dev | Malhotra | 1985-11-03 | MALE | O+ | 9876503002 | NULL | Leena Malhotra | 9876513002 | NULL | ACTIVE |
| PT26003 | Isha | Bose | 2001-02-19 | NOT_DISCLOSED | B- | 9876503003 | isha.bose@example.test | Tara Bose | 9876513003 | Peanuts | ACTIVE |
| PT26004 | Kiran | Ali | 1976-08-27 | INTERSEX | AB+ | 9876503004 | NULL | Sameer Ali | 9876513004 | NULL | INACTIVE |
| PT26005 | Neel | Joshi | 1990-06-10 | MALE | NULL | 9876503005 | neel.joshi@example.test | Maya Joshi | 9876513005 | Dust | ACTIVE |

1. **PAT-I1:** Insert the first patient using a single-row statement.
2. **PAT-I2:** Insert the second patient while preserving the optional `NULL` values.
3. **PAT-I3:** Insert the remaining three patients using one multi-row statement.
4. **PAT-I4:** Attempt an insert with blood group `X+`.
5. **PAT-I5:** Attempt an insert with biological-sex value `UNKNOWN`.
6. **PAT-I6:** Attempt to insert a duplicate patient number.

## B. UPDATE Exercises

1. **PAT-U1:** Add `Sulfa drugs` as the allergy note for `PT26002`.
2. **PAT-U2:** Add the email `dev.malhotra@example.test` for `PT26002`.
3. **PAT-U3:** Change the phone number of `PT26001` to `9876503991`.
4. **PAT-U4:** Set the blood group of `PT26005` to `O-`.
5. **PAT-U5:** Attempt to change a blood group to `C+` and confirm that the update fails.

## C. DELETE Exercises

1. **PAT-D1:** Preview and delete the inactive fictional patient `PT26004`.
2. **PAT-D2:** Insert a fictional temporary patient with number `PT-TEMP-01` and then delete it.
3. **PAT-D3:** Start a transaction, delete all active patient rows, verify the temporary result, and roll back.

> Production healthcare systems normally retain or archive patient records rather than physically deleting them. These deletions are strictly for SQL practice.

---

# 6. Bank Account DML Exercises

**Table:** `bank_accounts`

## A. INSERT Exercises

All account information is fictional. Allow primary-key and timestamp values to be generated automatically.

| Account number | Holder | Type | Balance | Currency | Branch | Opened date | Interest rate | Overdraft limit | Status |
|---|---|---|---:|---|---|---|---:|---:|---|
| 100000000001 | Aditi Sharma | SAVINGS | 85000.00 | INR | MG Road Branch | 2024-01-15 | 3.50 | 0.00 | ACTIVE |
| 100000000002 | Raj Enterprises | CURRENT | 450000.00 | INR | Commercial Street Branch | 2023-07-01 | 0.00 | 100000.00 | ACTIVE |
| 100000000003 | Priya Nair | FIXED_DEPOSIT | 300000.00 | INR | Kochi Main Branch | 2025-04-10 | 7.25 | 0.00 | ACTIVE |
| 100000000004 | Omar Khan | SAVINGS | 12500.00 | INR | Banjara Hills Branch | 2022-10-05 | 3.25 | 0.00 | FROZEN |
| 100000000005 | Training Closed Account | CURRENT | 0.00 | INR | Test Branch | 2020-01-01 | 0.00 | 0.00 | CLOSED |

1. **BNK-I1:** Insert the savings account using a single-row statement.
2. **BNK-I2:** Insert the current and fixed-deposit accounts using one multi-row statement.
3. **BNK-I3:** Insert the frozen and closed accounts using one multi-row statement.
4. **BNK-I4:** Attempt inserts having a negative balance, interest rate `101.00`, and account type `SALARY`.
5. **BNK-I5:** Attempt to insert a duplicate account number.

## B. UPDATE Exercises

1. **BNK-U1:** Deposit `25000.00` into account `100000000001` by adding it to the existing balance.
2. **BNK-U2:** Increase the interest rate of every savings account by `0.25`, ensuring it remains within the allowed range.
3. **BNK-U3:** Change account `100000000004` from `FROZEN` to `ACTIVE`.
4. **BNK-U4:** Withdraw `2500.00` from account `100000000004` only when its status is `ACTIVE` and its balance is sufficient.
5. **BNK-U5:** Rename `Commercial Street Branch` to `Central Business Branch`.
6. **BNK-U6:** Attempt a withdrawal that would create a negative balance and confirm that it is rejected or affects no row when guarded correctly.

## C. DELETE Exercises

1. **BNK-D1:** Preview and delete the fictional closed account having a zero balance.
2. **BNK-D2:** Insert a temporary account numbered `999999999999` and delete only that account.
3. **BNK-D3:** Start a transaction, delete all fixed-deposit accounts, inspect the result, and roll back.

> Real banking systems do not normally delete account history. These operations apply only to the fictional training table.

---

# 7. Vehicle DML Exercises

**Table:** `vehicles`

## A. INSERT Exercises

Allow the primary key and creation timestamp to be generated automatically.

| Registration number | Owner | Manufacturer | Model | Vehicle type | Fuel type | Year | Purchase date | Colour | Odometer km | Insurance expiry | Status |
|---|---|---|---|---|---|---:|---|---|---:|---|---|
| KA01AB1234 | Arjun Rao | Hyundai | Creta | CAR | DIESEL | 2022 | 2022-08-15 | White | 34000 | 2027-08-14 | ACTIVE |
| TS09CD5678 | Meera Iyer | Honda | Activa 6G | MOTORCYCLE | PETROL | 2021 | NULL | Red | 18500 | 2026-12-31 | ACTIVE |
| MH12EF9012 | Rohan Logistics | Tata | Ultra | TRUCK | DIESEL | 2020 | 2020-03-10 | Blue | 145000 | 2026-10-15 | IN_SERVICE |
| DL03GH3456 | Nisha Kapoor | Mahindra | eSupro | VAN | ELECTRIC | 2024 | 2024-02-01 | Silver | 22000 | NULL | ACTIVE |
| TN10JK7890 | Training Transport | Ashok Leyland | Viking | BUS | DIESEL | 2010 | NULL | Yellow | 480000 | NULL | SCRAPPED |

1. **VEH-I1:** Insert the car using a single-row statement.
2. **VEH-I2:** Insert the motorcycle and truck using one multi-row statement.
3. **VEH-I3:** Insert the van and bus, preserving their optional `NULL` values.
4. **VEH-I4:** Attempt inserts using a negative odometer value, vehicle type `SUV`, and fuel type `HYDROGEN`.
5. **VEH-I5:** Attempt to insert a duplicate registration number.

## B. UPDATE Exercises

1. **VEH-U1:** Add `750` kilometres to the odometer of `KA01AB1234`.
2. **VEH-U2:** Set the insurance expiry of `DL03GH3456` to `2027-02-01`.
3. **VEH-U3:** Change `MH12EF9012` from `IN_SERVICE` to `ACTIVE`.
4. **VEH-U4:** Change the colour of `TS09CD5678` to `Matte Red`.
5. **VEH-U5:** Add `1000` kilometres to every active vehicle's odometer.

## C. DELETE Exercises

1. **VEH-D1:** Preview and delete the fictional scrapped vehicle `TN10JK7890`.
2. **VEH-D2:** Insert a temporary vehicle with registration `TEST00TMP01` and then delete it.
3. **VEH-D3:** Start a transaction, delete vehicles having no insurance-expiry date, inspect the temporary result, and roll back.

---

# 8. Hotel Room DML Exercises

**Table:** `hotel_rooms`

## A. INSERT Exercises

Allow the primary key and timestamp columns to be generated automatically.

| Room number | Room type | Floor | Beds | Maximum occupancy | Price per night | Availability | Air conditioning | Smoking allowed | Notes |
|---|---|---:|---:|---:|---:|---|---|---|---|
| 101 | SINGLE | 1 | 1 | 1 | 2500.00 | AVAILABLE | TRUE | FALSE | NULL |
| 102 | DOUBLE | 1 | 2 | 3 | 4200.00 | OCCUPIED | TRUE | FALSE | City view |
| 201 | DELUXE | 2 | 1 | 2 | 6500.00 | RESERVED | TRUE | FALSE | Balcony |
| 301 | SUITE | 3 | 2 | 4 | 12000.00 | AVAILABLE | TRUE | FALSE | Sea view |
| T99 | SINGLE | 9 | 1 | 1 | 1000.00 | MAINTENANCE | FALSE | FALSE | Training room |

1. **ROM-I1:** Insert room `101` using a single-row statement.
2. **ROM-I2:** Insert rooms `102` and `201` using one multi-row statement.
3. **ROM-I3:** Insert room `301` and the training room using one multi-row statement.
4. **ROM-I4:** Attempt inserts having zero beds, zero occupancy, and zero price.
5. **ROM-I5:** Attempt an insert with availability `CLEANING` and another with a duplicate room number.

## B. UPDATE Exercises

1. **ROM-U1:** Increase the nightly price of every suite by `10%`.
2. **ROM-U2:** Change room `102` to `AVAILABLE` and set its notes to `Cleaning completed`.
3. **ROM-U3:** Increase room `201` maximum occupancy to `3` and price to `7000.00`.
4. **ROM-U4:** Keep room `T99` in maintenance and change its notes to `Scheduled for removal`.
5. **ROM-U5:** Attempt to change a room's maximum occupancy to `0` and confirm that the update fails.

## C. DELETE Exercises

1. **ROM-D1:** Preview and delete the training room `T99`.
2. **ROM-D2:** Insert a temporary room numbered `TMP1` and then delete it by room number.
3. **ROM-D3:** Start a transaction, delete all rooms on floor `3`, verify the result, and roll back.

---

# 9. Movie DML Exercises

**Table:** `movies`

## A. INSERT Exercises

The movie data below is fictional. Allow the primary key and timestamp columns to be generated automatically.

| Movie code | Title | Genre | Language | Release date | Duration | Director | Certificate | Rating | Budget | Status |
|---|---|---|---|---|---:|---|---|---:|---:|---|
| MOV26001 | River Beyond the Hills | Drama | Hindi | 2026-01-16 | 132 | Anika Verma | PARENTAL_GUIDANCE | 8.2 | 35000000.00 | RELEASED |
| MOV26002 | Orbit Seven | Science Fiction | English | 2026-05-22 | 148 | Daniel Cole | PARENTAL_GUIDANCE | 7.6 | 120000000.00 | RELEASED |
| MOV26003 | Little Mango Tree | Animation | Telugu | 2026-07-10 | 96 | Ravi Teja | ALL_AGES | 8.5 | 18000000.00 | RELEASED |
| MOV27001 | Echoes of Tomorrow | Thriller | English | NULL | 125 | Maya Sen | UNRATED | NULL | NULL | UPCOMING |
| MOV24005 | Old Harbour | Mystery | Bengali | 2024-02-09 | 118 | Sayan Dutta | ADULT | 6.9 | 22000000.00 | ARCHIVED |

1. **MOV-I1:** Insert the first movie using a single-row statement.
2. **MOV-I2:** Insert the next two released movies using one multi-row statement.
3. **MOV-I3:** Insert the upcoming and archived movies, preserving the unknown values as `NULL`.
4. **MOV-I4:** Attempt inserts with zero duration, rating `11.0`, and a negative budget.
5. **MOV-I5:** Attempt an insert with certificate `TEEN` and another using a duplicate movie code.

## B. UPDATE Exercises

1. **MOV-U1:** Set the release date of `MOV27001` to `2027-03-19` and its certificate to `PARENTAL_GUIDANCE`.
2. **MOV-U2:** Change the audience rating of `MOV26003` to `8.8`.
3. **MOV-U3:** Increase the production budget of every science-fiction movie by `5%` where the budget is known.
4. **MOV-U4:** Set catalogue status to `ARCHIVED` for released movies with a release date before `2025-01-01`.
5. **MOV-U5:** Attempt to change a movie rating to `12.0` and confirm that the update fails.

## C. DELETE Exercises

1. **MOV-D1:** Preview and delete the archived fictional movie `MOV24005`.
2. **MOV-D2:** Insert a temporary movie with code `MOV-TEMP-01` and then delete it.
3. **MOV-D3:** Start a transaction, delete every upcoming movie, inspect the temporary result, and roll back.

---

# 10. Support Ticket DML Exercises

**Table:** `support_tickets`

## A. INSERT Exercises

Allow `ticket_id`, `created_at`, and `last_updated_at` to be generated automatically. Use `CURRENT_TIMESTAMP` as the resolved time only where shown.

| Ticket number | Requester | Email | Subject | Description | Category | Priority | Status | Assigned agent | Resolved time |
|---|---|---|---|---|---|---|---|---|---|
| TKT-26001 | Asha Rao | asha.rao@example.test | Unable to reset password | Reset link is not arriving | ACCOUNT | HIGH | OPEN | NULL | NULL |
| TKT-26002 | Dev Stores | dev.stores@example.test | Incorrect invoice total | The latest invoice contains an extra charge | BILLING | MEDIUM | IN_PROGRESS | Neha | NULL |
| TKT-26003 | Meera Nair | meera.nair@example.test | Application crashes | Application closes while uploading a file | TECHNICAL | CRITICAL | OPEN | Vikram | NULL |
| TKT-26004 | Omar Ali | omar.ali@example.test | Change registered email | Request to replace the account email | ACCOUNT | LOW | OPEN | NULL | NULL |
| TKT-26005 | Test User | test.user@example.test | Sample resolved request | Temporary ticket used for delete practice | GENERAL | MEDIUM | RESOLVED | QA Agent | CURRENT_TIMESTAMP |

1. **TKT-I1:** Insert the first ticket using a single-row statement.
2. **TKT-I2:** Insert tickets `TKT-26002` and `TKT-26003` using one multi-row statement.
3. **TKT-I3:** Insert the remaining two tickets, using `NULL` and `CURRENT_TIMESTAMP` correctly.
4. **TKT-I4:** Attempt inserts using category `SHIPPING`, priority `URGENT`, and status `WAITING`.
5. **TKT-I5:** Attempt to insert a duplicate ticket number.
6. **TKT-I6:** Attempt a ticket with an explicit creation time of `2026-09-24 12:00:00` and resolved time `2026-09-24 11:00:00`. Confirm that the date check rejects it.

## B. UPDATE Exercises

1. **TKT-U1:** Assign `TKT-26001` to agent `Kavya` and change its status to `IN_PROGRESS`.
2. **TKT-U2:** Resolve `TKT-26003` by setting its status to `RESOLVED` and its resolved time to the current timestamp.
3. **TKT-U3:** Change the priority of all open account tickets from `LOW` to `MEDIUM`.
4. **TKT-U4:** Reassign `TKT-26002` from `Neha` to `Rahul`.
5. **TKT-U5:** Attempt to set a resolved time earlier than the creation time and confirm that the update fails.

## C. DELETE Exercises

1. **TKT-D1:** Preview and delete the temporary resolved ticket `TKT-26005`.
2. **TKT-D2:** Insert a ticket numbered `TKT-TEMP-01` and delete only that ticket.
3. **TKT-D3:** Start a transaction, delete all resolved tickets, inspect the result, and roll back.

> Production support systems commonly archive tickets instead of physically deleting them. These deletions are for SQL practice only.

---

# Final Mixed-DML Challenges

Complete these tasks after finishing all 10 sections:

1. Insert one additional valid row into each table using values of your own.
2. Insert three valid rows into one selected table using a single multi-row statement.
3. Use an expression in an `UPDATE`, such as increasing a price, balance, quantity, or rating.
4. Update two columns in the same statement.
5. Perform one update that intentionally matches multiple rows.
6. Perform one update whose guarded `WHERE` condition causes zero rows to change.
7. Insert and remove a temporary row from each table.
8. Execute one conditional delete inside a transaction and roll it back.
9. Demonstrate one constraint failure during insert and one during update.
10. Record the affected-row count for every successful DML statement.

# Submission Checklist

- [ ] Every `INSERT` uses an explicit column list.
- [ ] Auto-generated columns are omitted where appropriate.
- [ ] SQL `NULL` is not enclosed in quotation marks.
- [ ] Multi-row inserts use correct value ordering.
- [ ] Every update was previewed with a matching `SELECT` condition.
- [ ] Every delete was previewed with a matching `SELECT` condition.
- [ ] Single-row changes use a unique identifier in the `WHERE` clause.
- [ ] Multi-row changes have a documented reason.
- [ ] Constraint failures were recorded without weakening the table design.
- [ ] Risky operations were tested inside transactions.
- [ ] `ROLLBACK` and `COMMIT` were each demonstrated.
- [ ] Final table contents were verified after every section.

# Suggested Assessment Rubric

| Area | Marks |
|---|---:|
| Correct single-row inserts | 10 |
| Correct multi-row inserts | 10 |
| Handling optional values and defaults | 10 |
| Correct single-row updates | 10 |
| Correct conditional multi-row updates | 10 |
| Safe arithmetic updates | 10 |
| Correct targeted deletes | 10 |
| Transaction and rollback practice | 10 |
| Constraint-error observations | 10 |
| Verification, formatting, and safe `WHERE` usage | 10 |
| **Total** | **100** |
