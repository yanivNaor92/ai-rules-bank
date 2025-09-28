# Environment and Configuration Security

## Environment Variables and Secrets Management

### Secrets Management Best Practices
- Never hardcode secrets in source code
- Use environment variables for configuration
- Implement proper secrets rotation
- Use dedicated secrets management services
- Implement least privilege access to secrets

### Secure Environment Configuration
```javascript
// Good: Secure environment variable handling
const config = {
    // Database configuration
    database: {
        host: process.env.DB_HOST || 'localhost',
        port: parseInt(process.env.DB_PORT) || 5432,
        username: process.env.DB_USERNAME,
        password: process.env.DB_PASSWORD,
        name: process.env.DB_NAME,
        ssl: process.env.NODE_ENV === 'production'
    },
    
    // JWT configuration
    jwt: {
        accessSecret: process.env.JWT_ACCESS_SECRET,
        refreshSecret: process.env.JWT_REFRESH_SECRET,
        accessExpiresIn: process.env.JWT_ACCESS_EXPIRES_IN || '15m',
        refreshExpiresIn: process.env.JWT_REFRESH_EXPIRES_IN || '7d'
    },
    
    // Encryption keys
    encryption: {
        key: process.env.ENCRYPTION_KEY,
        algorithm: process.env.ENCRYPTION_ALGORITHM || 'aes-256-gcm'
    }
};

// Validate required environment variables
const requiredEnvVars = [
    'DB_HOST', 'DB_USERNAME', 'DB_PASSWORD', 'DB_NAME',
    'JWT_ACCESS_SECRET', 'JWT_REFRESH_SECRET', 'ENCRYPTION_KEY'
];

const missingEnvVars = requiredEnvVars.filter(envVar => !process.env[envVar]);

if (missingEnvVars.length > 0) {
    console.error('Missing required environment variables:', missingEnvVars);
    process.exit(1);
}

// Bad: Hardcoded secrets
const badConfig = {
    database: {
        password: 'mypassword123', // Never do this!
    },
    jwt: {
        secret: 'mysecretkey' // Never do this!
    }
};
```

## Secure Configuration Management

### Configuration Security Guidelines
- Separate configuration by environment (dev, staging, prod)
- Use configuration validation and type checking
- Implement configuration encryption for sensitive values
- Use immutable configuration objects
- Implement configuration auditing and versioning

### Configuration Validation
```javascript
// Good: Configuration validation with Joi
const Joi = require('joi');

const configSchema = Joi.object({
    NODE_ENV: Joi.string().valid('development', 'staging', 'production').required(),
    PORT: Joi.number().port().default(3000),
    
    // Database configuration
    DB_HOST: Joi.string().hostname().required(),
    DB_PORT: Joi.number().port().default(5432),
    DB_USERNAME: Joi.string().alphanum().min(3).max(30).required(),
    DB_PASSWORD: Joi.string().min(8).required(),
    DB_NAME: Joi.string().alphanum().min(1).max(50).required(),
    
    // JWT configuration
    JWT_ACCESS_SECRET: Joi.string().min(32).required(),
    JWT_REFRESH_SECRET: Joi.string().min(32).required(),
    JWT_ACCESS_EXPIRES_IN: Joi.string().default('15m'),
    JWT_REFRESH_EXPIRES_IN: Joi.string().default('7d'),
    
    // Encryption configuration
    ENCRYPTION_KEY: Joi.string().length(64).hex().required(),
    
    // Rate limiting
    RATE_LIMIT_WINDOW_MS: Joi.number().positive().default(900000), // 15 minutes
    RATE_LIMIT_MAX_REQUESTS: Joi.number().positive().default(100),
    
    // Security headers
    ENABLE_HSTS: Joi.boolean().default(true),
    ENABLE_CSP: Joi.boolean().default(true)
});

const { error, value: validatedConfig } = configSchema.validate(process.env, {
    allowUnknown: true,
    stripUnknown: true
});

if (error) {
    console.error('Configuration validation error:', error.details);
    process.exit(1);
}

module.exports = validatedConfig;
```

## Dependency Security

### Dependency Management Best Practices
- Regularly audit and update dependencies
- Use dependency vulnerability scanning tools
- Implement automated dependency updates
- Use lock files to ensure consistent installations
- Remove unused dependencies

### Dependency Security Implementation
```javascript
// Good: Package.json security configuration
{
    "name": "secure-user-api",
    "version": "1.0.0",
    "scripts": {
        "audit": "npm audit",
        "audit-fix": "npm audit fix",
        "security-check": "npm audit --audit-level moderate",
        "outdated": "npm outdated",
        "update-deps": "npm update"
    },
    "dependencies": {
        "express": "^4.18.2",
        "helmet": "^7.0.0",
        "bcrypt": "^5.1.0",
        "jsonwebtoken": "^9.0.0",
        "joi": "^17.9.0",
        "rate-limiter-flexible": "^2.4.1"
    },
    "devDependencies": {
        "audit-ci": "^6.6.1",
        "snyk": "^1.1161.0"
    }
}

// Automated security checks in CI/CD
// .github/workflows/security.yml
name: Security Audit
on: [push, pull_request]
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - run: npm audit --audit-level moderate
      - run: npx audit-ci --moderate
```

## Production Security Configuration

### Production Environment Security
- Disable debug modes and verbose logging
- Implement proper error handling without information disclosure
- Use production-grade session stores
- Implement proper monitoring and alerting
- Use secure communication protocols

### Production Security Middleware
```javascript
// Good: Production security middleware
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');

const productionSecurityMiddleware = (app) => {
    // Security headers
    app.use(helmet({
        contentSecurityPolicy: {
            directives: {
                defaultSrc: ["'self'"],
                styleSrc: ["'self'", "'unsafe-inline'"],
                scriptSrc: ["'self'"],
                imgSrc: ["'self'", "data:", "https:"],
                connectSrc: ["'self'"],
                fontSrc: ["'self'"],
                objectSrc: ["'none'"],
                mediaSrc: ["'self'"],
                frameSrc: ["'none'"]
            }
        },
        hsts: {
            maxAge: 31536000,
            includeSubDomains: true,
            preload: true
        }
    }));
    
    // Rate limiting
    const limiter = rateLimit({
        windowMs: 15 * 60 * 1000, // 15 minutes
        max: 100, // limit each IP to 100 requests per windowMs
        message: 'Too many requests from this IP',
        standardHeaders: true,
        legacyHeaders: false
    });
    app.use('/api/', limiter);
    
    // Strict rate limiting for auth endpoints
    const authLimiter = rateLimit({
        windowMs: 15 * 60 * 1000,
        max: 5, // limit each IP to 5 auth requests per windowMs
        skipSuccessfulRequests: true
    });
    app.use('/api/auth/', authLimiter);
    
    // Trust proxy (if behind reverse proxy)
    if (process.env.TRUST_PROXY === 'true') {
        app.set('trust proxy', 1);
    }
    
    // Disable x-powered-by header
    app.disable('x-powered-by');
};

module.exports = productionSecurityMiddleware;
```

## Logging and Monitoring Security

### Secure Logging Practices
- Never log sensitive information (passwords, tokens, PII)
- Implement structured logging with appropriate log levels
- Use centralized logging systems
- Implement log integrity protection
- Set up security event monitoring and alerting

### Security Logging Implementation
```javascript
// Good: Secure logging implementation
const winston = require('winston');
const crypto = require('crypto');

class SecurityLogger {
    constructor() {
        this.logger = winston.createLogger({
            level: process.env.LOG_LEVEL || 'info',
            format: winston.format.combine(
                winston.format.timestamp(),
                winston.format.errors({ stack: true }),
                winston.format.json()
            ),
            defaultMeta: { service: 'user-management-api' },
            transports: [
                new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
                new winston.transports.File({ filename: 'logs/security.log', level: 'warn' }),
                new winston.transports.File({ filename: 'logs/combined.log' })
            ]
        });
        
        if (process.env.NODE_ENV !== 'production') {
            this.logger.add(new winston.transports.Console({
                format: winston.format.simple()
            }));
        }
    }
    
    // Security event logging
    logSecurityEvent(event, details = {}) {
        const sanitizedDetails = this.sanitizeLogData(details);
        
        this.logger.warn('SECURITY_EVENT', {
            event,
            details: sanitizedDetails,
            timestamp: new Date().toISOString(),
            severity: 'HIGH'
        });
    }
    
    // Authentication logging
    logAuthEvent(event, userId, ip, userAgent, success = true) {
        this.logger.info('AUTH_EVENT', {
            event,
            userId: this.hashUserId(userId),
            ip: this.maskIP(ip),
            userAgent: this.sanitizeUserAgent(userAgent),
            success,
            timestamp: new Date().toISOString()
        });
    }
    
    // Sanitize sensitive data from logs
    sanitizeLogData(data) {
        const sensitiveFields = ['password', 'token', 'secret', 'key', 'ssn', 'creditCard'];
        const sanitized = { ...data };
        
        for (const field of sensitiveFields) {
            if (sanitized[field]) {
                sanitized[field] = '[REDACTED]';
            }
        }
        
        return sanitized;
    }
    
    // Hash user ID for privacy
    hashUserId(userId) {
        return crypto.createHash('sha256').update(userId.toString()).digest('hex').substring(0, 16);
    }
    
    // Mask IP address for privacy
    maskIP(ip) {
        const parts = ip.split('.');
        return parts.length === 4 ? `${parts[0]}.${parts[1]}.xxx.xxx` : 'xxx.xxx.xxx.xxx';
    }
    
    // Sanitize user agent
    sanitizeUserAgent(userAgent) {
        // Remove potentially sensitive information while keeping useful data
        return userAgent ? userAgent.substring(0, 200) : 'unknown';
    }
}

// Usage
const securityLogger = new SecurityLogger();

// Log failed login attempt
securityLogger.logSecurityEvent('FAILED_LOGIN_ATTEMPT', {
    ip: req.ip,
    userAgent: req.headers['user-agent'],
    attemptedEmail: 'user@example.com'
});

// Log successful authentication
securityLogger.logAuthEvent('LOGIN_SUCCESS', user.id, req.ip, req.headers['user-agent'], true);
```
