# SQL Injection Security Testing

## Objective and Scope

This section documents the execution and analysis of a **SQL Injection attack** against **Damn Vulnerable Web Application (DVWA)** deployed in a controlled **Kali Linux** laboratory environment. The objective was to demonstrate how SQL Injection can be used to manipulate database queries and access sensitive information, followed by an explanation of the appropriate mitigation techniques.

## 1. Vulnerability Description and Execution

### Vulnerability Discovered

The SQL Injection functionality in DVWA was tested at a low security level. The application was found to incorporate user-supplied input into a database query without sufficient protection, allowing the structure of the SQL query to be manipulated.

### Exploitation Process

The vulnerability was first confirmed by providing specially crafted input to the User ID field and observing the application's response.

The number of columns returned by the original query was then determined using SQL query manipulation. This information was used to construct a compatible **UNION SELECT** statement.

A UNION-based SQL Injection was subsequently performed to retrieve information from the application's database. The attack successfully demonstrated that database records containing user account information, including usernames and password hashes, could be exposed through the vulnerable functionality.

### Successful Data Extraction

The exploitation resulted in multiple database records being displayed through the application. The extracted information demonstrated that sensitive account data stored within the database could be accessed without using the application's intended authentication or authorization mechanisms.

**Evidence:** Screenshots of the injection process and extracted database information are included in the accompanying security-testing evidence.

## 2. Impact

The successful exploitation demonstrates that SQL Injection can allow an attacker to manipulate database queries and retrieve information that should normally remain inaccessible.

Depending on the application's database privileges and the information stored within the database, SQL Injection may result in exposure of:

* Usernames and account information
* Password hashes
* Application data
* Other sensitive database records

This makes SQL Injection a significant web application security vulnerability.

## 3. Mitigation and Prevention

The primary mitigation for SQL Injection is to prevent user input from being interpreted as part of an SQL statement.

### Vulnerable Implementation

The vulnerable approach directly incorporates user-controlled input into an SQL query:

```php
$user_input = $_GET['user_id'];
$query = "SELECT ... WHERE id = '$user_input'";
```

This approach allows specially crafted input to influence the structure of the query.

### Recommended Mitigation

Applications should use **Prepared Statements / Parameterized Queries** instead of directly concatenating user input into SQL statements.

For example:

```php
$stmt = $pdo->prepare("SELECT ... WHERE id = ?");
$stmt->execute([$user_input]);
```

With parameterized queries, user input is treated as data rather than executable SQL syntax. This prevents an attacker from modifying the intended structure of the database query.

Additional defensive measures can include appropriate input validation, least-privilege database accounts, secure error handling, and regular security testing.

## Conclusion

The DVWA exercise demonstrated the security risks associated with improperly handled database input. SQL Injection can allow attackers to manipulate application queries and access sensitive information. Using prepared statements and other secure development practices significantly reduces the risk of this vulnerability.
