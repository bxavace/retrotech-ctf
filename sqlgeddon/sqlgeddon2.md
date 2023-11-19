# SQLgeddon 2
Now that we know the MAC address, we need to know which user and which department the device belongs to, as it will be a future reference in the future on which department needs to be trained in the domain of ransomware attacks.

**Format: {email_department}**
(e.g., `RETROTECH{brylle@jissa.apc.edu.ph_Events}`)

# Solution
This is a follow-up question to the first SQLgeddon.

1. Since the player should already know the MAC address by now, they should also query for the user using relevant information.

```sql
    SELECT *
    FROM users
    WHERE user_id = <user_id>;
```

2. Once they saw the user email, they should now query for the department code. Example:

```sql
    SELECT d.department_name, u.first_name, u.last_name
    FROM users AS u
    JOIN departments AS d
        ON u.department_id = d.department_id
        WHERE u.first_name = '<first_name>' AND u.last_name = '<last_name>';
```

**Note:** Since this is OSINT, the problem will become easier should they try to find information visually, using existing compilers compared to finding information by querying textually (using SQL command line.)

**Flag:** `RETROTECH{maria.santos@jissa.net_HR}`