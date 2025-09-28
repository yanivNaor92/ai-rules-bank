# Data Privacy and Protection

## Personal Identifiable Information (PII) Handling

### PII Classification and Protection
- Identify and classify all PII in your system
- Implement data minimization principles
- Use encryption for sensitive PII storage
- Implement proper data retention policies
- Ensure secure data disposal methods

### PII Protection Implementation
```javascript
// Good: PII encryption and handling
const crypto = require('crypto');

class PIIHandler {
    constructor() {
        this.algorithm = 'aes-256-gcm';
        this.key = Buffer.from(process.env.PII_ENCRYPTION_KEY, 'hex');
    }
    
    encryptPII(data) {
        const iv = crypto.randomBytes(16);
        const cipher = crypto.createCipher(this.algorithm, this.key);
        cipher.setAAD(Buffer.from('pii-data'));
        
        let encrypted = cipher.update(data, 'utf8', 'hex');
        encrypted += cipher.final('hex');
        
        const authTag = cipher.getAuthTag();
        
        return {
            encrypted,
            iv: iv.toString('hex'),
            authTag: authTag.toString('hex')
        };
    }
    
    decryptPII(encryptedData) {
        const decipher = crypto.createDecipher(this.algorithm, this.key);
        decipher.setAAD(Buffer.from('pii-data'));
        decipher.setAuthTag(Buffer.from(encryptedData.authTag, 'hex'));
        
        let decrypted = decipher.update(encryptedData.encrypted, 'hex', 'utf8');
        decrypted += decipher.final('utf8');
        
        return decrypted;
    }
}

// Usage
const piiHandler = new PIIHandler();
const encryptedSSN = piiHandler.encryptPII(user.ssn);
```

## Data Anonymization and Pseudonymization

### Anonymization Techniques
- Remove direct identifiers (names, addresses, phone numbers)
- Use statistical disclosure control methods
- Implement k-anonymity for datasets
- Use differential privacy for statistical queries
- Implement proper data masking for non-production environments

### Data Masking Implementation
```javascript
// Good: Data masking for different data types
class DataMasker {
    static maskEmail(email) {
        const [username, domain] = email.split('@');
        const maskedUsername = username.charAt(0) + '*'.repeat(username.length - 2) + username.charAt(username.length - 1);
        return `${maskedUsername}@${domain}`;
    }
    
    static maskPhoneNumber(phone) {
        return phone.replace(/(\d{3})\d{3}(\d{4})/, '$1***$2');
    }
    
    static maskCreditCard(cardNumber) {
        return cardNumber.replace(/\d(?=\d{4})/g, '*');
    }
    
    static maskSSN(ssn) {
        return ssn.replace(/\d{3}-\d{2}-(\d{4})/, '***-**-$1');
    }
}

// Usage in non-production environments
const maskedUser = {
    ...user,
    email: DataMasker.maskEmail(user.email),
    phone: DataMasker.maskPhoneNumber(user.phone),
    ssn: DataMasker.maskSSN(user.ssn)
};
```

## GDPR and Privacy Compliance

### GDPR Requirements Implementation
- Implement data subject rights (access, rectification, erasure)
- Maintain data processing records
- Implement privacy by design principles
- Conduct data protection impact assessments
- Implement proper consent management

### Data Subject Rights Implementation
```javascript
// Good: GDPR data subject rights implementation
class GDPRCompliance {
    // Right to access (Article 15)
    async getPersonalData(userId) {
        const userData = await User.findById(userId);
        const userActivities = await Activity.findByUserId(userId);
        
        return {
            personalData: userData,
            activities: userActivities,
            dataRetention: await this.getRetentionPolicy(userId),
            thirdPartySharing: await this.getThirdPartySharing(userId)
        };
    }
    
    // Right to rectification (Article 16)
    async updatePersonalData(userId, updates) {
        // Validate updates
        const validatedUpdates = await this.validateDataUpdates(updates);
        
        // Log the change for audit purposes
        await this.logDataChange(userId, 'rectification', validatedUpdates);
        
        return await User.findByIdAndUpdate(userId, validatedUpdates);
    }
    
    // Right to erasure (Article 17)
    async deletePersonalData(userId, reason) {
        // Check if deletion is legally required
        if (!await this.canDeleteData(userId, reason)) {
            throw new Error('Data deletion not permitted under current legal basis');
        }
        
        // Anonymize instead of delete if required for legitimate interests
        if (await this.shouldAnonymizeInsteadOfDelete(userId)) {
            return await this.anonymizeUserData(userId);
        }
        
        // Log deletion for audit purposes
        await this.logDataDeletion(userId, reason);
        
        // Delete user data
        await User.findByIdAndDelete(userId);
        await Activity.deleteMany({ userId });
        
        return { deleted: true, timestamp: new Date() };
    }
    
    // Data portability (Article 20)
    async exportPersonalData(userId, format = 'json') {
        const personalData = await this.getPersonalData(userId);
        
        switch (format) {
            case 'json':
                return JSON.stringify(personalData, null, 2);
            case 'csv':
                return this.convertToCSV(personalData);
            case 'xml':
                return this.convertToXML(personalData);
            default:
                throw new Error('Unsupported export format');
        }
    }
}
```

## Consent Management

### Consent Implementation Guidelines
- Implement granular consent options
- Use clear and plain language for consent requests
- Implement consent withdrawal mechanisms
- Maintain consent records and timestamps
- Implement consent renewal processes

### Consent Management System
```javascript
// Good: Consent management implementation
class ConsentManager {
    async recordConsent(userId, consentType, granted, purpose) {
        const consent = {
            userId,
            consentType,
            granted,
            purpose,
            timestamp: new Date(),
            ipAddress: req.ip,
            userAgent: req.headers['user-agent'],
            version: this.getCurrentPrivacyPolicyVersion()
        };
        
        await Consent.create(consent);
        
        // Update user preferences
        await this.updateUserPreferences(userId, consentType, granted);
        
        return consent;
    }
    
    async withdrawConsent(userId, consentType) {
        await this.recordConsent(userId, consentType, false, 'withdrawal');
        
        // Stop processing based on withdrawn consent
        await this.stopProcessingForConsentType(userId, consentType);
        
        return { withdrawn: true, timestamp: new Date() };
    }
    
    async checkConsent(userId, consentType) {
        const latestConsent = await Consent.findOne({
            userId,
            consentType
        }).sort({ timestamp: -1 });
        
        if (!latestConsent) {
            return false;
        }
        
        // Check if consent is still valid (not expired)
        const isValid = await this.isConsentValid(latestConsent);
        
        return latestConsent.granted && isValid;
    }
}
```

## Data Retention and Disposal

### Data Retention Policies
- Define retention periods for different data types
- Implement automated data deletion processes
- Maintain audit logs of data disposal
- Implement secure data destruction methods
- Regular review and update of retention policies

### Automated Data Retention Implementation
```javascript
// Good: Automated data retention system
class DataRetentionManager {
    constructor() {
        this.retentionPolicies = {
            'user_activity_logs': 90, // days
            'session_data': 30,
            'audit_logs': 2555, // 7 years
            'marketing_data': 1095, // 3 years
            'inactive_accounts': 1825 // 5 years
        };
    }
    
    async enforceRetentionPolicies() {
        for (const [dataType, retentionDays] of Object.entries(this.retentionPolicies)) {
            await this.deleteExpiredData(dataType, retentionDays);
        }
    }
    
    async deleteExpiredData(dataType, retentionDays) {
        const cutoffDate = new Date();
        cutoffDate.setDate(cutoffDate.getDate() - retentionDays);
        
        const deletedCount = await this.performSecureDeletion(dataType, cutoffDate);
        
        // Log retention enforcement
        await this.logRetentionEnforcement(dataType, deletedCount, cutoffDate);
        
        return deletedCount;
    }
    
    async performSecureDeletion(dataType, cutoffDate) {
        // Implement secure deletion based on data type
        switch (dataType) {
            case 'user_activity_logs':
                return await ActivityLog.deleteMany({ 
                    createdAt: { $lt: cutoffDate } 
                });
            case 'session_data':
                return await Session.deleteMany({ 
                    lastAccessed: { $lt: cutoffDate } 
                });
            // Add other data types...
        }
    }
}

// Schedule retention enforcement
const retentionManager = new DataRetentionManager();
setInterval(() => {
    retentionManager.enforceRetentionPolicies();
}, 24 * 60 * 60 * 1000); // Daily
```
