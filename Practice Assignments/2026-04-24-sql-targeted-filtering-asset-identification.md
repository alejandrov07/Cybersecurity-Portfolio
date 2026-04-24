# Precision Data Filtering: Targeted Asset Compliance and Incident Investigation

## Executive Summary
This operation involved applying advanced SQL filtering techniques to isolate specific organizational assets and personnel data. The primary objective was to facilitate targeted patch management for systems running legacy operating systems and to perform a localized incident investigation for hardware failures reported in specific physical locations.

## Technical Environment
* **Database Management System:** MariaDB
* **Key Operators:** `WHERE`, `LIKE`, `%` (Wildcard)
* **Target Tables:** `machines`, `employees`

## Operational Procedures

### Phase 1: Targeted OS Patching Audit
To streamline the remediation of systems running vulnerable software, I filtered the fleet inventory to isolate high-priority assets.

1.  **Legacy OS Identification:** Isolated all devices running 'OS 2' to prepare for a coordinated security update. This targeted approach prevents "alert fatigue" by excluding non-relevant assets.
    ```sql
    SELECT device_id, operating_system 
    FROM machines 
    WHERE operating_system = 'OS 2';
    ```

### Phase 2: Departmental Compliance Notification
The organization required a security awareness distribution specifically for departments handling sensitive financial data.

1.  **Finance & Sales Scoping:** Extracted personnel records for specific high-risk departments to ensure receipt of confidentiality protocols.
    ```sql
    -- Scoping Finance Department
    SELECT * FROM employees WHERE department = 'Finance';

    -- Scoping Sales Department
    SELECT * FROM employees WHERE department = 'Sales';
    ```

### Phase 3: Incident Response & Location-Based Investigation
Following a reported hardware issue in a specific facility ("South Building"), I performed a query-based investigation to identify affected users.

1.  **Point-of-Failure Identification:** Identified the specific employee assigned to a workstation in office 'South-109' to initiate technical support.
    ```sql
    SELECT * FROM employees WHERE office = 'South-109';
    ```
2.  **Building-Wide Scope via Pattern Matching:** Leveraged wildcards to identify all personnel within the 'South' building, anticipating a wider infrastructure failure.
    ```sql
    SELECT * FROM employees 
    WHERE office LIKE 'South%';
    ```

## Security Insight
Granular filtering is a core competency for **Security Operations Center (SOC)** analysts. Utilizing the `WHERE` clause minimizes "data noise," allowing for rapid response during time-critical events. Furthermore, the use of the `LIKE` operator and wildcards (`%`) is indispensable for **Threat Hunting** and log analysis, particularly when investigating lateral movement or identifying assets within a specific network subnet or physical location based on partial string data.

*This project is part of my cybersecurity portfolio [SQL SECURITY AUDITING]*
