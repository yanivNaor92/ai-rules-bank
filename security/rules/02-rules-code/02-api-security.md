# Secure API Design

## API Security Principles

### Authentication and Authorization
- Implement proper API authentication (OAuth 2.0, JWT, API keys)
- Use HTTPS for all API communications
- Implement rate limiting to prevent abuse
- Use proper HTTP status codes
- Implement API versioning for security updates

### Input Validation
- Validate all API input parameters
- Implement proper request size limits
- Use schema validation for request bodies
- Sanitize and validate file uploads
- Implement proper error handling without information disclosure

### Response Security
- Don't expose sensitive information in API responses
- Implement proper CORS policies
- Use appropriate HTTP headers for security
- Implement response size limits
- Use proper content-type headers

## Common API Security Patterns

### API Key Management
```javascript
// Good: Secure API key validation
const validateApiKey = (apiKey) => {
    if (!apiKey || typeof apiKey !== 'string') {
        return false;
    }
    
    // Validate format and check against database
    const keyPattern = /^[a-zA-Z0-9]{32,}$/;
    return keyPattern.test(apiKey) && isValidKeyInDatabase(apiKey);
};
```

### Rate Limiting Implementation
```javascript
// Good: Rate limiting with proper headers
const rateLimitMiddleware = (req, res, next) => {
    const clientId = req.headers['x-api-key'] || req.ip;
    const requests = getRequestCount(clientId);
    
    if (requests > RATE_LIMIT) {
        res.setHeader('X-RateLimit-Limit', RATE_LIMIT);
        res.setHeader('X-RateLimit-Remaining', 0);
        res.setHeader('X-RateLimit-Reset', getResetTime());
        return res.status(429).json({ error: 'Rate limit exceeded' });
    }
    
    next();
};
```

### Input Sanitization
```javascript
// Good: Comprehensive input sanitization
const sanitizeInput = (input) => {
    if (typeof input !== 'string') {
        return input;
    }
    
    return input
        .trim()
        .replace(/[<>]/g, '') // Remove potential HTML tags
        .replace(/['"]/g, '') // Remove quotes
        .substring(0, 1000); // Limit length
};
```

## Security Headers

### Essential Security Headers
- `Strict-Transport-Security`: Enforce HTTPS
- `Content-Security-Policy`: Prevent XSS attacks
- `X-Frame-Options`: Prevent clickjacking
- `X-Content-Type-Options`: Prevent MIME sniffing
- `Referrer-Policy`: Control referrer information

### Implementation Example
```javascript
// Good: Comprehensive security headers
app.use((req, res, next) => {
    res.setHeader('Strict-Transport-Security', 'max-age=31536000; includeSubDomains');
    res.setHeader('Content-Security-Policy', "default-src 'self'");
    res.setHeader('X-Frame-Options', 'DENY');
    res.setHeader('X-Content-Type-Options', 'nosniff');
    res.setHeader('Referrer-Policy', 'strict-origin-when-cross-origin');
    next();
});
```

## Error Handling

### Secure Error Responses
- Don't expose internal system information
- Use generic error messages for users
- Log detailed errors server-side only
- Implement proper error codes
- Don't reveal system paths or stack traces

### Error Handling Pattern
```javascript
// Good: Secure error handling
const handleError = (error, req, res, next) => {
    // Log detailed error for debugging
    logger.error('API Error:', {
        error: error.message,
        stack: error.stack,
        url: req.url,
        method: req.method,
        ip: req.ip
    });
    
    // Return generic error to client
    res.status(500).json({
        error: 'Internal server error',
        code: 'INTERNAL_ERROR'
    });
};
```
