````markdown
# SQL Injection Security Testing

## Objective and Scope

This section documents the execution and analysis of a **SQL Injection vulnerability** against **Damn Vulnerable Web Application (DVWA)** deployed in a controlled **Kali Linux** laboratory environment. The objective was to demonstrate how improperly handled user input can alter the logic of a database query and to document appropriate mitigation techniques.

## 1. Vulnerability Description and Execution

### Vulnerability Discovered

The SQL Injection functionality in DVWA was tested at the **Low** security level. The User ID input was found to accept specially crafted SQL expressions that could alter the behavior of the underlying database query.

The SQL Injection functionality uses a query structured around the supplied User ID:

```sql
SELECT first_name, last_name FROM users WHERE user_id = $id
````

Because user-controlled input can influence the query, specially crafted input can change the conditions applied by the database.

### Exploitation Process

The vulnerability was first tested using a normal User ID:

```text
1
```

The application returned:

```text
ID: 1
First name: admin
Surname: admin
```

This established the normal behavior of the application.

A SQL Injection test was then performed using:

```text
1' OR '1'='1
```

Instead of returning only the record associated with User ID `1`, the application returned **multiple user records**. This demonstrated that the supplied input was able to alter the logic of the database query.

This provided evidence that the User ID parameter was vulnerable to SQL Injection.

### UNION Testing

A UNION-based test was also attempted:

```text
1 UNION SELECT 1,2
```

However, the application displayed the supplied input rather than producing a successful UNION-based result. Therefore, **UNION-based database extraction was not considered a successful part of the test**.

The testing was consequently limited to the SQL Injection behavior that could be directly demonstrated and verified through the application.

## 2. Impact

The successful injection demonstrated that an attacker could manipulate the logic of the database query through user-controlled input.

Depending on the application's database permissions and implementation, SQL Injection can potentially result in:

* Unauthorized access to database records
* Exposure of user and application information
* Modification or deletion of database data
* Authentication bypass
* Exposure of sensitive information stored by the application

In this test, the demonstrated impact was the ability to alter the query logic and cause **multiple database records to be returned instead of the expected single record**.

## 3. Mitigation and Prevention

The primary mitigation for SQL Injection is to ensure that user input is never interpreted as part of the SQL statement.

### Vulnerable Implementation

The vulnerable query structure directly incorporates the supplied User ID:

```php
$id = $_GET['id'];
$query = "SELECT first_name, last_name FROM users WHERE user_id = $id";
```

Because the input is incorporated directly into the query, specially crafted input can influence the query's logic.

### Recommended Mitigation

Applications should use **Prepared Statements / Parameterized Queries**:

```php
$stmt = $pdo->prepare(
    "SELECT first_name, last_name FROM users WHERE user_id = ?"
);

$stmt->execute([$id]);
```

With a parameterized query, the SQL structure is defined separately from the user-supplied value. The database therefore treats the supplied value as **data rather than executable SQL syntax**.

Additional defensive measures include:

* Validating input according to the expected data type
* Using least-privilege database accounts
* Avoiding detailed database error messages in production
* Performing regular security testing

## Conclusion

The DVWA exercise successfully demonstrated a **SQL Injection vulnerability** in the User ID functionality. A normal input returned a single user record, while crafted SQL input altered the query behavior and resulted in multiple records being returned.

Although UNION-based extraction was tested, it was **not successfully demonstrated** in this environment and was therefore not presented as a successful finding.

The vulnerability can be mitigated primarily through **prepared statements or parameterized queries**, supported by input validation, least-privilege database access, and secure error handling.

```
```
