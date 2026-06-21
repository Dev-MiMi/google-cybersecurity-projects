# Apply Filters to SQL Queries

*April 2026*

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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dbb30ac2-6b46-44c4-a710-db2c0c747db6" />

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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e8164cad-3d69-4d31-9214-8dd33b0e9ed6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/89d8d48f-4b43-4290-9d1b-529a37e7e810" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5b43b16c-974a-4706-81c9-14fb563b24ef" />

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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0bf1b8aa-3934-418d-8675-495bc78d45b1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/065af22a-b0b5-4cad-96fd-5b6c8709232c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3e02d0c9-aae1-4592-a1bc-6e9f53a15483" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/53c08715-1e87-4bbe-a2dc-f87046196eea" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/97dbdf28-5cd7-4437-8f3e-6b5438e6dbf7" />

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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f8246b75-5118-4841-a383-00bc9ab0e135" />

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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/80ee0a65-aa30-4271-9181-259edf2bc877" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/40d31ac0-f6e5-41c0-a0d9-5e80bc37f908" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/526d746b-af20-4433-b99f-37de867d21f1" />


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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/27c969b5-6d49-47df-9090-70503d7d995b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/03386af9-40c0-4f1b-a82f-ef7e4b3dcc54" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/63e2ce76-acd5-4b7b-8e3f-c4586f587044" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3bfa55b2-9807-4b6d-b4ae-2afde22adcc6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b7538c7f-088f-4c46-b9eb-0c399ca228dc" />

---

## Summary

In this activity, I used SQL filters to investigate two potential security incidents and support machine update operations. By applying the `AND`, `OR`, `NOT`, and `LIKE` operators, I was able to retrieve precisely targeted subsets of data from the `log_in_attempts` and `employees` tables. Specifically, I filtered failed after-hours logins, isolated activity on suspicious dates, excluded logins from Mexico using a wildcard pattern, and identified groups of employees by department and office location. These SQL skills are essential for a security analyst role, enabling fast, accurate data retrieval that drives informed decisions during security investigations.
