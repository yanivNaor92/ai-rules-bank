# Core Security Principles

## Fundamental Security Guidelines

When developing or reviewing code, always prioritize these core security principles:

### 1. Security by Design
- Integrate security considerations from the initial design phase
- Consider threat models and attack vectors early in development
- Design systems with security controls as fundamental components
- Never treat security as an afterthought or add-on feature

### 2. Defense in Depth
- Implement multiple layers of security controls
- Don't rely on a single security mechanism
- Create redundant security measures that can catch failures
- Design systems to remain secure even if one layer fails

### 3. Least Privilege Principle
- Grant users and processes only the minimum permissions necessary
- Limit access rights to the smallest scope possible
- Regularly review and revoke unnecessary permissions
- Use role-based access control (RBAC) where appropriate

### 4. Fail Secure
- Systems should fail in a secure state by default
- Error conditions should not expose sensitive information
- Implement proper error handling that doesn't leak system details
- Default to denying access when in doubt

### 5. Input Validation and Sanitization
- Never trust user input - validate and sanitize all data
- Use whitelist validation over blacklist validation
- Implement proper encoding and escaping mechanisms
- Validate data at both client and server sides

### 6. Secure Communication
- Use encryption for all sensitive data transmission
- Implement proper certificate validation
- Use secure protocols (HTTPS, TLS) for all communications
- Never transmit sensitive data in plain text

### 7. Regular Security Updates
- Keep all dependencies and libraries up to date
- Monitor security advisories for used components
- Implement automated security scanning in CI/CD pipelines
- Have a process for applying security patches quickly
