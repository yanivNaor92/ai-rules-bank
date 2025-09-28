# Web Application Security

## Cross-Site Request Forgery (CSRF) Protection

### CSRF Prevention Guidelines
- Implement CSRF tokens for all state-changing operations
- Use SameSite cookie attributes
- Validate origin and referrer headers
- Implement proper token validation on server-side
- Use double-submit cookie pattern for stateless applications

### Implementation Example
```javascript
// Good: CSRF token validation
const validateCSRFToken = (req, res, next) => {
    const token = req.headers['x-csrf-token'] || req.body._csrf;
    const sessionToken = req.session.csrfToken;
    
    if (!token || !sessionToken || token !== sessionToken) {
        return res.status(403).json({ error: 'Invalid CSRF token' });
    }
    
    next();
};

// Apply to all POST, PUT, DELETE routes
app.use('/api', validateCSRFToken);
```

## Cross-Origin Resource Sharing (CORS)

### CORS Security Guidelines
- Configure CORS policies restrictively
- Avoid using wildcard (*) for origins in production
- Specify allowed methods and headers explicitly
- Implement proper preflight request handling
- Use credentials flag carefully

### Secure CORS Configuration
```javascript
// Good: Restrictive CORS configuration
const corsOptions = {
    origin: ['https://yourdomain.com', 'https://app.yourdomain.com'],
    methods: ['GET', 'POST', 'PUT', 'DELETE'],
    allowedHeaders: ['Content-Type', 'Authorization', 'X-CSRF-Token'],
    credentials: true,
    maxAge: 86400 // 24 hours
};

app.use(cors(corsOptions));

// Bad: Overly permissive CORS
app.use(cors({
    origin: '*',
    credentials: true
}));
```

## Cross-Site Scripting (XSS) Prevention

### XSS Protection Strategies
- Implement Content Security Policy (CSP) headers
- Use proper output encoding and escaping
- Validate and sanitize all user input
- Use templating engines that auto-escape
- Implement proper cookie security flags

### Content Security Policy Implementation
```javascript
// Good: Strict CSP policy
app.use((req, res, next) => {
    res.setHeader('Content-Security-Policy', 
        "default-src 'self'; " +
        "script-src 'self' 'unsafe-inline'; " +
        "style-src 'self' 'unsafe-inline'; " +
        "img-src 'self' data: https:; " +
        "connect-src 'self'; " +
        "font-src 'self'; " +
        "object-src 'none'; " +
        "media-src 'self'; " +
        "frame-src 'none';"
    );
    next();
});
```

## Session Security

### Secure Session Configuration
- Use secure session storage mechanisms
- Implement proper session timeout
- Use secure and httpOnly cookie flags
- Implement session regeneration after login
- Use strong session ID generation

### Session Implementation
```javascript
// Good: Secure session configuration
app.use(session({
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: false,
    cookie: {
        secure: process.env.NODE_ENV === 'production', // HTTPS only in production
        httpOnly: true, // Prevent XSS access to cookies
        maxAge: 1800000, // 30 minutes
        sameSite: 'strict' // CSRF protection
    },
    store: new RedisStore({
        host: process.env.REDIS_HOST,
        port: process.env.REDIS_PORT
    })
}));
```

## Security Headers

### Essential Security Headers
- `X-Frame-Options`: Prevent clickjacking
- `X-Content-Type-Options`: Prevent MIME sniffing
- `Referrer-Policy`: Control referrer information
- `Permissions-Policy`: Control browser features
- `Strict-Transport-Security`: Enforce HTTPS

### Security Headers Implementation
```javascript
// Good: Comprehensive security headers
app.use((req, res, next) => {
    res.setHeader('X-Frame-Options', 'DENY');
    res.setHeader('X-Content-Type-Options', 'nosniff');
    res.setHeader('Referrer-Policy', 'strict-origin-when-cross-origin');
    res.setHeader('Permissions-Policy', 'camera=(), microphone=(), geolocation=()');
    res.setHeader('X-XSS-Protection', '1; mode=block');
    
    if (process.env.NODE_ENV === 'production') {
        res.setHeader('Strict-Transport-Security', 'max-age=31536000; includeSubDomains');
    }
    
    next();
});
```
