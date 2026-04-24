# Relational Database Auditing: Asset Inventory and Authentication Analysis

## Executive Summary
This operational activity focused on leveraging SQL queries to perform two critical Blue Team functions: maintaining asset visibility for vulnerability management and auditing authentication logs for behavioral anomalies. By querying the `organization` database, I identified hardware assets requiring security patches and scrutinized login patterns to establish a security baseline.

## Technical Environment
* **Database Management System:** MariaDB / MySQL
* **Dataset:** `organization` database (Tables: `machines`, `log_in_attempts`)
* **Focus Areas:** Asset Management, SIEM-like manual log analysis, and Security Auditing.

## Operational Procedures

### Phase 1: Patch Management & Asset Inventory
To mitigate the risk of exploitation through outdated software, I performed a targeted audit of the organization's fleet.

1.  **Full Fleet Visibility:** Retrieved all records from the `machines` table to assess the current state of infrastructure.
    ```sql
    SELECT * FROM machines;
    ```
2.  **Email Client Exposure:** Isolated device IDs and their respective email clients to identify potentially vulnerable software versions across the network.
    ```sql
    SELECT device_id, email_client FROM machines;
    ```
3.  **Patch Compliance Audit:** Filtered for OS versions and last patch dates to prioritize systems requiring immediate remediation.
    ```sql
    SELECT device_id, operating_system, OS_patch_date FROM machines;
    ```

### Phase 2: Authentication Log Investigation
I analyzed the `log_in_attempts` table to detect indicators of compromise (IoCs) or unauthorized access attempts.

1.  **Geographic Anomaly Detection:** Verified login origins to ensure compliance with authorized operating regions (US, Canada, Mexico).
    ```sql
    SELECT event_id, country FROM log_in_attempts;
    ```
2.  **Temporal Analysis:** Audited timestamps to identify "after-hours" access, which often correlates with credential stuffing or insider threats.
    ```sql
    SELECT username, login_date, login_time FROM log_in_attempts;
    ```

### Phase 3: Forensic Sorting and Sequencing
To facilitate chronological event reconstruction, I applied sorting logic to the authentication data.

1.  **Timeline Reconstruction:** Ordered all login attempts by date and time to visualize the sequence of access events across the organization.
    ```sql
    SELECT * FROM log_in_attempts 
    ORDER BY login_date, login_time;
    ```

## Security Insight
From a defensive standpoint, the ability to query raw data is the first line of defense in **Incident Response**. Manually auditing `OS_patch_date` allows for the identification of "shadow IT" or neglected assets that bypass automated scanners. Furthermore, sequencing login attempts by time (`ORDER BY`) is a fundamental forensic technique used to identify "brute force" patterns or lateral movement within a compromised environment.

*This project is part of my cybersecurity portfolio [SQL SECURITY AUDITING]*
