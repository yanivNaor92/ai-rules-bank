# Secure Coding Practices

## Input Validation and Sanitization

### Validation Principles
- Validate all input data at the point of entry
- Use whitelist validation over blacklist validation
- Implement both client-side and server-side validation
- Validate data types, ranges, and formats
- Reject invalid input rather than attempting to fix it

### Sanitization Guidelines
- Escape special characters in output
- Use proper encoding for different contexts (HTML, URL, SQL)
- Implement output encoding based on context
- Use templating engines that auto-escape by default
- Never trust data from external sources

### Common Validation Patterns
```javascript
// Good: Whitelist validation
const allowedChars = /^[a-zA-Z0-9\s\-_]+$/;
if (!allowedChars.test(userInput)) {
    throw new Error('Invalid characters detected');
}

// Bad: Blacklist validation
if (userInput.includes('<script>')) {
    // This can be bypassed easily
}
```

## Authentication and Authorization

### Authentication Best Practices
- Use strong password policies (minimum 12 characters, complexity requirements)
- Implement multi-factor authentication (MFA) for sensitive accounts
- Use secure password hashing (bcrypt, Argon2, scrypt)
- Implement account lockout after failed attempts
- Use secure session management with proper timeouts

### Authorization Patterns
- Implement role-based access control (RBAC)
- Use principle of least privilege
- Validate permissions on every request
- Don't rely on client-side authorization
- Implement proper session invalidation

### Session Management
- Use secure, random session identifiers
- Implement proper session timeout
- Store session data securely
- Implement session fixation protection
- Use HTTPS for all session-related communications

## Data Protection

### Encryption Guidelines
- Encrypt sensitive data at rest using strong algorithms (AES-256)
- Use TLS 1.3 for data in transit
- Implement proper key management
- Use different keys for different purposes
- Regularly rotate encryption keys

### Data Handling
- Minimize data collection and retention
- Implement data classification
- Use secure data disposal methods
- Implement proper backup encryption
- Follow data privacy regulations (GDPR, CCPA)

### Sensitive Data Protection
- Never log sensitive information
- Use tokenization for sensitive data storage
- Implement data masking in non-production environments
- Use secure communication channels
- Implement proper access controls for sensitive data
