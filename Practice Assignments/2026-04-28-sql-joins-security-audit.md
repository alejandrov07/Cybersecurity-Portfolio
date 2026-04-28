# Relational Data Correlation for Incident Response via SQL Joins

## Security Insight
From an Identity and Access Management (IAM) and Asset Management perspective, the ability to correlate users with specific hardware is fundamental for accountability. During this audit, the use of `INNER JOIN` allowed for the identification of authorized pairings, while `LEFT` and `RIGHT` joins exposed critical security gaps: specifically, unassigned machines. These "orphaned" assets represent a high risk, as they can be utilized by unauthorized actors or insiders to bypass logging controls, effectively acting as "shadow devices" within the network perimeter.

## Technical Execution

### 1. Mapping Assets to Identities
To establish the baseline of known-user to known-device, an inner join was performed between the `machines` and `employees` tables using the `device_id` as the primary key for correlation.

```sql
SELECT * FROM machines 
INNER JOIN employees ON machines.device_id = employees.device_id;
```

### 2. Identifying Unmanaged Assets (Shadow IT)
By executing a `LEFT JOIN`, I identified machines that exist in the inventory but lack an assigned user (returning `NULL` in the username column). This is a primary indicator of poor asset lifecycle management.

```sql
SELECT * FROM machines 
LEFT JOIN employees ON machines.device_id = employees.device_id;
```

Conversely, a `RIGHT JOIN` was used to identify employees without assigned hardware, which helps in auditing seat-license compliance and identifying potential ghost accounts.

```sql
SELECT * FROM machines 
RIGHT JOIN employees ON machines.device_id = employees.device_id;
```

### 3. Login Attempt Correlation
To investigate potential brute-force or unauthorized access patterns, I correlated the employee directory with the login logs. This returned 200 records, providing a complete audit trail of user activity.

```sql
SELECT * FROM employees 
INNER JOIN log_in_attempts ON employees.username = log_in_attempts.username;
```

*This project is part of my cybersecurity portfolio THREAT_HUNTING_SQL*
