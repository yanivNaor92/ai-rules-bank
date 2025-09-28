# Authentication and Authorization

## Password Security

### Password Policy Requirements
- Minimum 12 characters length
- Require combination of uppercase, lowercase, numbers, and special characters
- Implement password strength validation
- Prevent common passwords and dictionary words
- Implement password history to prevent reuse

### Secure Password Hashing
```javascript
// Good: Using bcrypt for password hashing
const bcrypt = require('bcrypt');
const SALT_ROUNDS = 12;

const hashPassword = async (password) => {
    // Validate password strength first
    if (!isStrongPassword(password)) {
        throw new Error('Password does not meet security requirements');
    }
    
    return await bcrypt.hash(password, SALT_ROUNDS);
};

const verifyPassword = async (password, hash) => {
    return await bcrypt.compare(password, hash);
};

// Bad: Using weak hashing or plain text
const weakHash = crypto.createHash('md5').update(password).digest('hex');
```

## JWT (JSON Web Token) Security

### JWT Best Practices
- Use strong, randomly generated secrets
- Implement proper token expiration (short-lived access tokens)
- Use refresh tokens for long-term authentication
- Validate tokens on every request
- Implement proper token revocation mechanisms

### Secure JWT Implementation
```javascript
// Good: Secure JWT configuration
const jwt = require('jsonwebtoken');

const generateTokens = (user) => {
    const accessToken = jwt.sign(
        { 
            userId: user.id, 
            email: user.email,
            role: user.role 
        },
        process.env.JWT_ACCESS_SECRET,
        { 
            expiresIn: '15m',
            issuer: 'your-app-name',
            audience: 'your-app-users'
        }
    );
    
    const refreshToken = jwt.sign(
        { userId: user.id },
        process.env.JWT_REFRESH_SECRET,
        { 
            expiresIn: '7d',
            issuer: 'your-app-name',
            audience: 'your-app-users'
        }
    );
    
    return { accessToken, refreshToken };
};

const verifyToken = (token, secret) => {
    try {
        return jwt.verify(token, secret, {
            issuer: 'your-app-name',
            audience: 'your-app-users'
        });
    } catch (error) {
        throw new Error('Invalid token');
    }
};
```

## Multi-Factor Authentication (MFA)

### MFA Implementation Guidelines
- Implement TOTP (Time-based One-Time Password) for 2FA
- Support backup codes for account recovery
- Require MFA for privileged accounts
- Implement proper MFA bypass protection
- Use secure QR code generation for setup

### TOTP Implementation Example
```javascript
// Good: TOTP implementation
const speakeasy = require('speakeasy');
const qrcode = require('qrcode');

const generateMFASecret = async (user) => {
    const secret = speakeasy.generateSecret({
        name: `YourApp (${user.email})`,
        issuer: 'YourApp'
    });
    
    // Store secret securely in database (encrypted)
    await storeEncryptedSecret(user.id, secret.base32);
    
    // Generate QR code for user setup
    const qrCodeUrl = await qrcode.toDataURL(secret.otpauth_url);
    
    return {
        secret: secret.base32,
        qrCode: qrCodeUrl
    };
};

const verifyMFAToken = async (userId, token) => {
    const secret = await getEncryptedSecret(userId);
    
    return speakeasy.totp.verify({
        secret: secret,
        encoding: 'base32',
        token: token,
        window: 1 // Allow 1 step tolerance
    });
};
```

## Role-Based Access Control (RBAC)

### RBAC Implementation Guidelines
- Define clear roles and permissions
- Implement principle of least privilege
- Use hierarchical role structures when appropriate
- Implement proper permission checking middleware
- Regularly audit and review permissions

### RBAC Middleware Example
```javascript
// Good: RBAC middleware implementation
const checkPermission = (requiredPermission) => {
    return async (req, res, next) => {
        try {
            const user = req.user; // From authentication middleware
            
            if (!user) {
                return res.status(401).json({ error: 'Authentication required' });
            }
            
            const userPermissions = await getUserPermissions(user.id);
            
            if (!userPermissions.includes(requiredPermission)) {
                return res.status(403).json({ error: 'Insufficient permissions' });
            }
            
            next();
        } catch (error) {
            return res.status(500).json({ error: 'Authorization check failed' });
        }
    };
};

// Usage
app.get('/admin/users', 
    authenticateToken, 
    checkPermission('users:read'), 
    getUsersHandler
);

app.delete('/admin/users/:id', 
    authenticateToken, 
    checkPermission('users:delete'), 
    deleteUserHandler
);
```

## Account Security

### Account Protection Measures
- Implement account lockout after failed login attempts
- Use progressive delays for failed attempts
- Implement CAPTCHA for suspicious activities
- Monitor for unusual login patterns
- Implement secure password reset flows

### Account Lockout Implementation
```javascript
// Good: Account lockout with progressive delays
const MAX_LOGIN_ATTEMPTS = 5;
const LOCKOUT_TIME = 15 * 60 * 1000; // 15 minutes

const handleLoginAttempt = async (email, password) => {
    const user = await getUserByEmail(email);
    
    if (!user) {
        // Prevent user enumeration
        await simulatePasswordCheck();
        throw new Error('Invalid credentials');
    }
    
    // Check if account is locked
    if (user.lockoutUntil && user.lockoutUntil > Date.now()) {
        throw new Error('Account temporarily locked');
    }
    
    const isValidPassword = await verifyPassword(password, user.passwordHash);
    
    if (!isValidPassword) {
        await incrementFailedAttempts(user.id);
        throw new Error('Invalid credentials');
    }
    
    // Reset failed attempts on successful login
    await resetFailedAttempts(user.id);
    
    return user;
};
```
