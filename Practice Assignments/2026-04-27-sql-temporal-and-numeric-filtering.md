# Temporal and Numeric Filtering for Security Incident Response

## Security Insight
Timely identification of anomalies requires precise filtering of timestamps and event identifiers. In this audit, we focus on "Early Bird" anomalies—logins occurring before standard business hours (07:00:00). Such patterns are often indicative of unauthorized internal activity or automated scripts executing during low-traffic periods to avoid detection. By leveraging numeric ranges (Event IDs) and date boundaries, we can effectively isolate the blast radius of a potential security breach.

## Technical Execution

### 1. Post-Incident Timeline Analysis
Retrieving all login activity following a confirmed incident date to track lateral movement or persistence.
```sql
-- Initial broad search
SELECT * FROM log_in_attempts 
WHERE login_date >= '2022-05-09';

-- Narrowed window for deep forensics
SELECT * FROM log_in_attempts 
WHERE login_date BETWEEN '2022-05-09' AND '2022-05-11';
```

### 2. Pre-Shift Activity Monitoring (Insider Threat Hunting)
Isolating logins before 07:00:00, specifically focusing on the 06:00:00-07:00:00 window to identify users present before official supervision starts.
```sql
SELECT * FROM log_in_attempts 
WHERE login_time BETWEEN '06:00:00' AND '07:00:00';
```

### 3. Event ID Range Filtering
Filtering by specific `event_id` sequences to track related system events. Numeric data is treated without quotes to maintain query integrity.
```sql
SELECT event_id, username, login_date 
FROM log_in_attempts 
WHERE event_id BETWEEN 100 AND 150;
```

*This project is part of my cybersecurity portfolio [SQL-AUDIT]*
