# Apply Filters to SQL Queries

*Cybersecurity Portfolio Activity | April 2026*

---

## Project Description

As a security professional at a large organization, I was tasked with investigating potential security incidents involving unauthorized login attempts and employee machine vulnerabilities. Using SQL, I queried two core database tables — `log_in_attempts` and `employees` — to retrieve and analyze relevant records. I applied a combination of filtering operators, including `AND`, `OR`, `NOT`, and `LIKE`, to isolate specific subsets of data that required further security attention. The queries produced in this activity demonstrate practical use of SQL filters to support security investigations and inform decisions about system updates.

---

## Retrieve After Hours Failed Login Attempts

A potential security incident was discovered that occurred after business hours. To investigate, I queried the `log_in_attempts` table to identify all failed login attempts that took place after 18:00.

### SQL Query

```sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00' AND success = FALSE;
```

### Explanation

This query uses the `AND` operator to combine two conditions: the `login_time` must be greater than `'18:00'` (i.e., after 6 PM), and the `success` column must equal `FALSE`, which represents a failed login attempt. Both conditions must be true simultaneously for a record to be returned. The result set shows all employees who attempted — and failed — to log in outside of normal business hours, which is a key indicator of potential unauthorized access.

---

## Retrieve Login Attempts on Specific Dates

A suspicious event occurred on `2022-05-09`. To investigate, I needed to review all login activity on that day and the day before (`2022-05-08`).

### SQL Query

```sql
SELECT *
FROM log_in_attempts
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
```

### Explanation

This query uses the `OR` operator to return records matching either of two dates: `2022-05-09` or `2022-05-08`. Unlike `AND` (which requires both conditions to be true), `OR` returns a row if at least one condition is satisfied. Filtering for dates uses the format `'YYYY-MM-DD'` in single quotes, which matches the date data type in the `login_date` column. The query returned **75 login attempts** across the two days, which were then reviewed for anomalies.

---

## Retrieve Login Attempts Outside of Mexico

The security team determined that the suspicious activity did not originate in Mexico. I needed to find all login attempts from every country except Mexico, noting that the `country` column stores values as both `'MEX'` and `'MEXICO'`.

### SQL Query

```sql
SELECT *
FROM log_in_attempts
WHERE NOT country LIKE 'MEX%';
```

### Explanation

This query uses the `NOT` operator combined with `LIKE` to exclude records where the `country` column begins with `'MEX'`. The wildcard character `%` matches any sequence of characters that follows, so `'MEX%'` matches both `'MEX'` and `'MEXICO'`. Using `LIKE` with a pattern is essential here because the data is not stored in a single consistent format. `NOT` negates the condition, ensuring only rows where the country does NOT start with `'MEX'` are returned. The query identified **144 login attempts** originating outside of Mexico.

---

## Retrieve Employees in Marketing

The security team needed to update machines for employees in the Marketing department located in the East building. The `office` column contains values like `'East-170'` and `'East-320'`, so a pattern match was required.

### SQL Query

```sql
SELECT *
FROM employees
WHERE department = 'Marketing' AND office LIKE 'East%';
```

### Explanation

This query uses the `AND` operator to filter on two separate columns simultaneously: `department` must equal `'Marketing'` exactly, and `office` must begin with `'East'`. The `LIKE` keyword with the `%` wildcard captures all East building offices regardless of the room number suffix (e.g., East-170, East-320). Both conditions must be true for a record to appear in the results. The first employee returned was `elarson`.

---

## Retrieve Employees in Finance or Sales

A different security update was required for all machines belonging to employees in either the Finance or Sales departments.

### SQL Query

```sql
SELECT *
FROM employees
WHERE department = 'Finance' OR department = 'Sales';
```

### Explanation

This query uses the `OR` operator to retrieve employees who belong to either the Finance or Sales department. Even though both conditions reference the same column (`department`), SQL requires each condition to be written out in full — simply writing `department = 'Finance' OR 'Sales'` would not work correctly. `OR` returns a record if at least one of the conditions is met, so all employees from either department are included. The first Sales department employee returned was `lrodriqu`.

---

## Retrieve All Employees Not in IT

A final machine update was required for all employees except those already updated in the Information Technology department. I used the `NOT` operator to exclude IT employees.

### SQL Query

```sql
SELECT *
FROM employees
WHERE NOT department = 'Information Technology';
```

### Explanation

This query uses the `NOT` operator to negate the condition `department = 'Information Technology'`, returning every employee who does NOT belong to that department. `NOT` is placed directly before the condition it negates, making the query readable and easy to adapt. This approach is more efficient than listing every other department individually. The query returned **161 employees** who are eligible for the pending security update.

---

## Summary

In this activity, I used SQL filters to investigate two potential security incidents and support machine update operations. By applying the `AND`, `OR`, `NOT`, and `LIKE` operators, I was able to retrieve precisely targeted subsets of data from the `log_in_attempts` and `employees` tables. Specifically, I filtered failed after-hours logins, isolated activity on suspicious dates, excluded logins from Mexico using a wildcard pattern, and identified groups of employees by department and office location. These SQL skills are essential for a security analyst role, enabling fast, accurate data retrieval that drives informed decisions during security investigations.
