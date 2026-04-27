# Advanced SQL Filtering for Security Incident Investigation

## Security Insight
As a security analyst, multi-vector filtering is essential for identifying anomalous patterns that deviate from established baselines. Investigating failed logins outside of business hours (post-18:00) combined with geographic filtering (Origin: NOT MEX) allows for the detection of potential brute-force attacks or unauthorized access attempts from foreign jurisdictions. This logic reduces "noise" and isolates high-risk events where actors leverage off-peak hours to minimize the probability of real-time intervention by SOC teams.

## Technical Execution

### 1. After-Hours Failed Login Analysis
Filtering for unsuccessful attempts (success = 0) occurring after 18:00 to identify potential brute-force activity.
```sql
SELECT * FROM log_in_attempts 
WHERE login_time > '18:00' AND success = 0;
```

### 2. Specific Date Correlation
Retrieving all activity for a specific 48-hour window to correlate with reported suspicious events.
```sql
SELECT * FROM log_in_attempts 
WHERE login_date = '2022-05-08' OR login_date = '2022-05-09';
```

### 3. Geographic Anomaly Detection
Isolating login attempts originating from outside the organization's primary operational region (Mexico).
```sql
SELECT * FROM log_in_attempts 
WHERE NOT country LIKE 'MEX%';
```

### 4. Departmental Access Audits
Segmenting employees by department and office location to ensure hardware updates and access control compliance.

**Marketing in East Building:**
```sql
SELECT * FROM employees 
WHERE department = 'Marketing' AND office LIKE 'East%';
```

**Finance or Sales Departments:**
```sql
SELECT * FROM employees 
WHERE department = 'Finance' OR department = 'Sales';
```

**Excluding IT Department for Delta Updates:**
```sql
SELECT * FROM employees 
WHERE NOT department = 'Information Technology';
```

*This project is part of my cybersecurity portfolio [SQL-AUDIT]*
