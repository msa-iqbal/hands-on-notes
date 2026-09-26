# Web Security

Practical, structured notes for understanding **web application security, common web vulnerabilities, attack techniques, security controls, and secure web development practices**.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Web Security Fundamentals|Web security concepts, attack surfaces, trust boundaries, browser-server communication, and common risks|
|02|OWASP Top 10|Major web application security risks, vulnerability categories, prevention, and secure development practices|
|03|SQL Injection|SQL injection concepts, attack vectors, vulnerable queries, prevention, parameterized queries, and testing|
|04|XSS|Cross-site scripting, reflected XSS, stored XSS, DOM-based XSS, prevention, and output encoding|
|05|CSRF|Cross-site request forgery, attack conditions, defenses, CSRF tokens, SameSite cookies, and validation|
|06|SSRF|Server-side request forgery, request manipulation, internal resources, validation, and mitigation|
|07|Clickjacking|Frame-based attacks, UI redressing, framing controls, and browser security protections|
|08|File Upload Security|Unsafe uploads, file validation, storage, execution risks, content inspection, and secure upload handling|
|09|Command Injection|Command injection risks, unsafe command execution, input handling, process isolation, and prevention|
|10|Path Traversal|Directory traversal, file-access manipulation, path validation, canonicalization, and mitigation|
|11|Security Headers|HTTP security headers, browser protections, configuration, and security policy enforcement|
|12|Web Security Best Practices|Secure input handling, output encoding, authentication, session security, dependency management, and security testing|

## Structure

The notes progress from **web security fundamentals → OWASP Top 10 → common web vulnerabilities → security headers → web security best practices**.

The first sections establish the security model of web applications and introduce the major risk categories commonly encountered during web development. The vulnerability sections then examine specific attack classes, including SQL injection, XSS, CSRF, SSRF, clickjacking, unsafe file uploads, command injection, and path traversal.

Security headers introduce browser-level protections and HTTP security policies that help reduce common attack surfaces. The final section brings these concepts together into practical secure-development practices covering input handling, authentication, sessions, dependencies, configuration, and testing.

Each topic is separated into focused notes for easier learning, revision, vulnerability analysis, mitigation, and secure web development.

## Focus

- Web security fundamentals
- OWASP Top 10
- Common web vulnerabilities
- Secure file and command handling
- HTTP security headers
- Vulnerability mitigation
- Secure web development

## Goal

> Build a practical web security knowledge base that makes it easy to understand common web vulnerabilities, recognize attack surfaces, apply appropriate mitigations, and develop more secure web applications.
