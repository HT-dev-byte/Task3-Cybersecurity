# Web Security Headers Analysis and Hardening

## Objective and Scope

This section documents the analysis and hardening of HTTP security headers for **Damn Vulnerable Web Application (DVWA)** in a controlled **Kali Linux** laboratory environment.

The objective was to identify missing or incorrectly configured security headers, understand their security implications, and demonstrate how appropriate headers can be implemented to improve the security posture of the web application.

## 1. Baseline Analysis

### Security Header Assessment

A baseline assessment of the DVWA application was performed using a security-header analysis method. The assessment was used to identify HTTP security headers that were missing, incorrectly configured, or could be improved.

The initial analysis indicated that several recommended security controls were not fully implemented.

### Vulnerability Identified

Security headers provide instructions to web browsers about how application content should be handled. When these headers are absent or incorrectly configured, the application may have increased exposure to browser-based attacks such as clickjacking, MIME-type confusion, and certain forms of content injection.

## 2. Mitigation and Hardening

The Apache web server configuration was reviewed and appropriate security headers were added to HTTP responses.

The Apache headers functionality was enabled where required, and security-related directives were added to the web server configuration.

Examples of security headers considered during the hardening process included:

```apache
Header always set X-Frame-Options "DENY"
Header always set X-Content-Type-Options "nosniff"
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
```

A suitable **Content-Security-Policy (CSP)** can also be configured according to the requirements of the application.

The web server was subsequently reloaded or restarted so that the configuration changes could take effect.

## 3. Purpose of Security Headers

| Security Header               | Purpose                                                                                                                          |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **X-Frame-Options**           | Helps prevent unauthorized framing of the application and reduces the risk of clickjacking.                                      |
| **X-Content-Type-Options**    | Prevents browsers from unnecessarily interpreting content as a different MIME type.                                              |
| **Strict-Transport-Security** | Helps enforce HTTPS connections when HTTPS is properly configured.                                                               |
| **Content-Security-Policy**   | Restricts the sources from which browser resources can be loaded and provides additional protection against client-side attacks. |

The exact headers required depend on the application's functionality and deployment environment.

## 4. Verification

Following the configuration changes, the application's HTTP responses were inspected again to verify the presence and configuration of the security headers.

The results were compared with the initial assessment to determine whether the intended security controls had been successfully implemented.

## 5. Conclusion

The security-header assessment demonstrated the importance of configuring appropriate HTTP security controls at the web server level.

Security headers provide an additional layer of defense by allowing the browser to enforce restrictions on how application content is loaded and displayed. However, they should complement rather than replace secure application development practices.

Combining security headers with proper input validation, output encoding, authentication, authorization, and secure session management provides a stronger overall web application security posture.
