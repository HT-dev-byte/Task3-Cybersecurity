# Web Security Analysis

## Objective and Scope

This section documents the analysis of the web security configuration for **Damn Vulnerable Web Application (DVWA)** in a controlled **Kali Linux** laboratory environment.

The objective was to inspect the application's HTTP responses and identify security-related configuration issues.

## 1. Baseline Analysis

The DVWA application's HTTP responses were inspected to examine the security-related information returned by the web server.

The assessment was used to identify areas where the application's existing security configuration could be improved.

## 2. Analysis

The HTTP response information was reviewed using **Burp Suite** while interacting with the DVWA application.

The captured requests and responses provided information about how the application communicated with the browser and how its web server handled incoming requests.

This analysis provided an additional perspective on the application's security configuration alongside the vulnerability testing performed for SQL Injection, XSS, and CSRF.

## 3. Verification

The relevant DVWA requests and responses were inspected through Burp Suite during testing.

**Evidence:** Screenshots of the captured HTTP requests and responses are included as supporting evidence.

## 4. Conclusion

The exercise demonstrated the usefulness of inspecting HTTP traffic when assessing a web application's security configuration. **Burp Suite** provided visibility into the communication between the browser and DVWA, allowing requests and responses to be examined during the security-testing process.
