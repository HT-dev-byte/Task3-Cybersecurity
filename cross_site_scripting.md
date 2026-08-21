# Cross-Site Scripting Security Testing

## Objective and Scope

This section documents the testing and analysis of **Reflected Cross-Site Scripting (XSS)** and **Stored Cross-Site Scripting (XSS)** against **Damn Vulnerable Web Application (DVWA)** in a controlled **Kali Linux** laboratory environment.

The objective was to demonstrate how insufficient input handling can allow user-supplied content to be interpreted as executable browser-side code and to document appropriate mitigation techniques.

## 1. Reflected XSS

### Vulnerability and Execution

Reflected XSS occurs when user-supplied input is returned directly in an application's response without appropriate output encoding. If the input is interpreted as HTML or JavaScript by the browser, the supplied script may execute within the context of the affected page.

**Target:** A DVWA functionality that reflects user-supplied input back into the page.

**Test Payload:**

```html
<script>alert('XSS Test')</script>
```

### Result

The test payload was submitted through the vulnerable functionality, and the browser displayed the JavaScript alert. This demonstrated that the supplied script was interpreted and executed rather than being treated purely as text.

This confirmed a **Reflected XSS vulnerability** in the tested functionality.

Because reflected XSS is not normally stored by the application, exploitation generally requires the victim to interact with a specially crafted request or link.

## 2. Stored XSS

### Vulnerability and Execution

Stored XSS occurs when user-supplied content is stored by an application and subsequently displayed without appropriate input validation or output encoding.

**Target:** A DVWA functionality designed to store user-supplied content.

**Test Payload:**

```html
<script>alert('Stored XSS Test')</script>
```

### Result

The test input was submitted through the vulnerable functionality and persisted by the application. When the affected content was subsequently displayed, the browser executed the supplied script.

This demonstrated that the malicious input persisted within the application and could execute again when the affected content was viewed.

## 3. Impact

XSS vulnerabilities can allow attacker-controlled JavaScript to execute within a user's browser in the context of the vulnerable application.

Depending on the application's functionality and security controls, potential consequences include:

* Unauthorized actions performed within the user's session
* Manipulation of displayed web content
* Phishing or social-engineering attacks
* Exposure of information accessible to client-side scripts
* Session-related attacks where appropriate protections are absent

The severity depends on the application's functionality and the security controls protecting user sessions and sensitive information.

## 4. Mitigation and Prevention

The primary defenses against XSS are **input validation** and, most importantly, **context-appropriate output encoding**.

User-supplied content should always be treated as untrusted data and should not be inserted into an HTML response without appropriate encoding.

Additional protections include:

* Validate input according to the expected data format.
* Apply appropriate output encoding before displaying user-controlled content.
* Use frameworks and templating systems that provide automatic HTML escaping.
* Implement a suitable **Content Security Policy (CSP)** as an additional defense-in-depth measure.
* Configure session cookies with appropriate security attributes such as `HttpOnly` and `Secure` where applicable.

## Conclusion

The Reflected and Stored XSS exercises demonstrated the risks associated with processing untrusted user input without appropriate output encoding.

**Reflected XSS** occurs when malicious input is immediately returned to the browser, whereas **Stored XSS** occurs when the input is persisted and subsequently executed when the affected content is viewed.

Treating user input as untrusted data, applying appropriate output encoding, validating input, and implementing additional controls such as CSP significantly reduces the risk of XSS attacks.
