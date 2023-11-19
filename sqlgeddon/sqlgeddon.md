# SQLgeddon 1
Category: HARD, `2199` points

PRE-REQUISITES: Basic Data Manipulation (DML) in SQL (e.g., commands like `SELECT`, `WHERE`, `IN`, `LIKE`, etc.)

You are a database security analyst at JISSA Branch TIPQC. The organization experienced a cyberattack. You need to track the affected device needed to be analyzed and submit its **MAC address** to the forensics team. 

You had the report but you lost it. The only way to find it is through querying the log of incidents in the access log database. 

All you know is that the incident happened in the **month of September afternoon** and it was allegedly a **Ransom attack**.

Note: Finishing this challenge will unlock a follow-up challenge of the same difficulty.

> *Proficiency in SQL is crucial for cybersecurity and OSINT enthusiasts as it enables effective data analysis, allowing them to query databases for threat detection, incident response, and vulnerability assessment. SQL skills are essential for tasks such as log analysis, custom reporting, and ensuring database security, empowering professionals to navigate and extract insights from large datasets in the realm of cybersecurity and open-source intelligence.*

# Solution
This problem is a test of proficiency in querying and extracting data from a relational database given a small information.

1. To begin with, it's better for the user to see all tables and analyze the entries to give them context about the problem. Example code below:

```sql
    .schema table_name -- (in server command line)
    -- or
    PRAGMA table_info("incidents");
    -- or
    SELECT sql 
    FROM sqlite_schema 
    WHERE name = 'devices';
```

2. We know that the incident happened at the month of September, no date but timestamp was given -- it was around afternoon. Therefore, going through the `incidents` table and finding related records. An example command would be:

```sql
    SELECT *
    FROM incidents
        WHERE MONTH(timestamp) = 9
        AND HOUR(timestamp) >= 12
        AND HOUR(timestamp) < 18
        AND attack_type_id = <attack_type_id>;
```

2. Since we need to find the MAC address, we know that a record from `incidents` will return the `incident_id`, `attack_type_id`, `device_id`, `user_id`, and `access_log_id`. Given the information, we could find relevant data like the below:

```sql
    SELECT *
    FROM devices
    WHERE device_id = <device_id>;
```

3. Same method (3) can be done for finding the virus name.
4. Now, examine the results and they should see the MAC address and virus name of the affected device.

**Flag**: `RETROTECH{1D:76:58:2D:B8:78,neuralNemesis.exe}`