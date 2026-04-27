# Advanced SQL Filtering for Incident Response and Log Analysis

## Executive Summary
During a security incident investigation, the ability to isolate specific events from massive datasets is critical for timely remediation. This project demonstrates the application of SQL filtering techniques to analyze login attempts, identify out-of-hours activity, and isolate specific event IDs within an organizational database. By leveraging comparison operators and range filters, I successfully identified suspicious patterns that warrant further forensic investigation.

## Technical Context
The investigation was conducted on the `log_in_attempts` table within a MariaDB environment. The primary objective was to filter raw log data into actionable intelligence based on specific temporal and numerical parameters.

### Security Insight
From a Blue Team perspective, filtering by specific time ranges (e.g., `BETWEEN '06:00:00' AND '07:00:00'`) is essential for identifying anomalous behavior, such as automated brute-force attacks or unauthorized access by compromised accounts outside of standard business hours. Narrowing the scope reduces "analytical noise" and allows for the correlation of events with known threat actor TTPs (Tactics, Techniques, and Procedures).

## Log Analysis & Query Execution

### 1. Chronological Filtering
To identify attempts after a known baseline of security updates, I filtered for logins following a specific threshold:

```sql
SELECT * FROM log_in_attempts 
WHERE login_date >= '2022-05-09';
