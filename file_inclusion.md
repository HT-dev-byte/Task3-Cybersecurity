# Local File Inclusion Security Testing

## Objective and Scope

This section documents the testing and analysis of a **Local File Inclusion (LFI)** vulnerability against **Damn Vulnerable Web Application (DVWA)** in a controlled Kali Linux laboratory environment.

The objective was to demonstrate how improperly handled file paths can allow an attacker to access files outside the intended application directory and to document the secure coding practices required to mitigate the vulnerability.

## 1. Vulnerability Description and Execution

### Vulnerability Discovered

The tested file-loading functionality accepts user-controlled input to determine which file or resource should be loaded. Without sufficient validation and restrictions on the supplied path, an attacker may manipulate the input to reference files outside the intended application directory.

### Exploitation Process

The vulnerable file-loading parameter was first identified through the application's File Inclusion functionality.

A **directory traversal** technique was then used to modify the requested path. Directory traversal sequences allow an attacker to move upward through the server's directory structure and potentially access files outside the application's intended directory.

A commonly used test target in a Linux-based laboratory environment is:

```text id="9j4n1m"
/etc/passwd
```

This file contains information about local user accounts and is useful for demonstrating whether the application can access files outside its intended directory.

### Result

The file inclusion test successfully demonstrated that the application could be manipulated into accessing a local file outside the expected application directory.

**Evidence:** Screenshots of the vulnerable functionality, the test request, and the resulting file contents are included in the security-testing evidence.

## 2. Impact

Successful LFI exploitation can allow an attacker to read files that should not be accessible through the web application.

Depending on the permissions of the web server and the application's implementation, potential consequences can include exposure of:

* System information
* Application configuration files
* Source code
* Credentials or configuration data
* User account information
* Other sensitive local files

The severity depends on which files are accessible and the privileges of the application process.

## 3. Mitigation and Prevention

The strongest defense against LFI is to avoid allowing users to directly specify arbitrary file paths.

### Whitelisting

The application should maintain an explicit list of permitted files or resources. User input should select from predefined values rather than being treated as an arbitrary filesystem path.

For example:

```text id="g8n5xk"
Allowed:
home
about
contact

Rejected:
../../etc/passwd
../../../configuration
```

### Path Validation

If user-controlled paths are unavoidable, the application should:

* Validate the requested path.
* Resolve the canonical path before accessing the filesystem.
* Ensure the resolved path remains inside the intended application directory.
* Reject paths that escape the approved directory.
* Avoid directly passing untrusted input to file inclusion functions.

Simply removing strings such as `../` is not sufficient on its own because attackers may use alternative representations or encoding techniques to bypass simplistic filters.

### Least Privilege

The web application should also run with the minimum filesystem permissions required for its operation. This limits the potential impact if an LFI vulnerability is successfully exploited.

## Conclusion

The LFI exercise demonstrated how insufficient validation of file paths can allow an attacker to access files outside the intended application directory.

Restricting file selection through whitelisting, validating canonical paths, preventing directory traversal, and applying least-privilege filesystem permissions can significantly reduce the risk and impact of Local File Inclusion vulnerabilities.
