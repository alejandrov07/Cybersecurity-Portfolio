# Filtering Incident Logs via SQL Numerical and Temporal Operators

## Security Insight
In incident response, precision is paramount. Querying broad datasets introduces "noise" and increases Mean Time to Detect (MTTD). By leveraging SQL comparison operators ($>$, $<$, $=$) and temporal constraints (BETWEEN), an analyst can isolate unauthorized access patterns. Specifically, monitoring the 06:00:00 to 07:00:00 window targets potential "Shadow Hour" attacks, where adversaries attempt to blend in with early-bird automated processes or exploit the gap during security shift handovers. Identifying high `event_id` sequences within these windows is a primary method for detecting brute-force persistence or lateral movement.

## Technical Execution Summary
This project involved auditing the `log_in_attempts` table to identify suspicious activity based on specific temporal and numerical triggers.

* **Date-Based Filtering:** Isolated login attempts occurring after a specific breach notification date ('2022-05-09') using the `>` and `>=` operators.
* **Range Constraints:** Utilized the `BETWEEN` and `AND` operators to define a strict 48-hour investigation window for log correlation.
* **Temporal Analysis:** Executed sub-hour queries (06:00 to 07:00) to detect anomalous user behavior outside of standard business hours.
* **Identifier Audit:** Filtered specific `event_id` sequences to map authentication failures and successes, identifying high-frequency events associated with targeted accounts.

## Schema & Query Samples
| Objective | SQL Syntax (Conceptual) |
| :--- | :--- |
| **Post-Incident Audit** | `SELECT * FROM log_in_attempts WHERE login_date > '2022-05-09';` |
| **Window Isolation** | `SELECT * FROM log_in_attempts WHERE login_date BETWEEN '2022-05-09' AND '2022-05-11';` |
| **Shadow Hour Detection** | `SELECT * FROM log_in_attempts WHERE login_time BETWEEN '06:00:00' AND '07:00:00';` |
| **Numeric Filtering** | `SELECT event_id, username FROM log_in_attempts WHERE event_id >= 100;` |

## Conclusion
The investigation successfully narrowed down 186 total logs to a subset of critical events, identifying specific users (e.g., `bisles`) active during high-risk intervals. Mastering these filters is essential for maintaining a clean audit trail and performing efficient forensic analysis in MariaDB/MySQL environments.

This project is part of my cybersecurity portfolio [SQL FOR DEFENSIVE OPERATIONS]
