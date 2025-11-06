# Security Best Practices

## Purpose
Security is everyone's responsibility. This guide provides practical security practices for all engineers to follow when building, deploying, and maintaining our applications.

## Core Principles

1. **Defense in Depth** - Multiple layers of security
2. **Least Privilege** - Minimal access necessary
3. **Fail Securely** - Errors shouldn't expose vulnerabilities
4. **Security by Design** - Not an afterthought
5. **Continuous Vigilance** - Security is ongoing, not one-time

---

## Authentication & Authorization

### Password Security

❌ **NEVER:**
```javascript
// Storing passwords in plaintext
user.password = req.body.password;

// Weak hashing
const hash = md5(password);
const hash = sha1(password);
```

✅ **ALWAYS:**
```javascript
// Use bcrypt with sufficient rounds (10-12)
const bcrypt = require('bcrypt');
const saltRounds = 12;
const hashedPassword = await bcrypt.hash(password, saltRounds);

// Verify passwords securely
const isValid = await bcrypt.compare(password, hashedPassword);
```

### Password Requirements
- Minimum 12 characters
- Require: uppercase, lowercase, number, special character
- Check against common password lists
- Implement rate limiting on login attempts
- Support password managers (no weird paste restrictions)

### Token Management

**JWT Best Practices:**
```javascript
// ✅ Short-lived access tokens
const accessToken = jwt.sign(
  { userId: user.id, role: user.role },
  process.env.JWT_SECRET,
  { expiresIn: '15m' } // 15 minutes
);

// ✅ Longer-lived refresh tokens (stored securely)
const refreshToken = jwt.sign(
  { userId: user.id, tokenVersion: user.tokenVersion },
  process.env.REFRESH_TOKEN_SECRET,
  { expiresIn: '7d' }
);

// ✅ Store refresh tokens in httpOnly, secure cookies
res.cookie('refreshToken', refreshToken, {
  httpOnly: true,
  secure: process.env.NODE_ENV === 'production',
  sameSite: 'strict',
  maxAge: 7 * 24 * 60 * 60 * 1000
});
```

❌ **Don't:**
- Store tokens in localStorage (XSS vulnerable)
- Use weak or default secrets
- Set expiresIn to years
- Include sensitive data in tokens

### Authorization Checks

```javascript
// ✅ Check permissions on every request
const requireAuth = (req, res, next) => {
  if (!req.user) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  next();
};

// ✅ Check resource ownership
const requireOwnership = async (req, res, next) => {
  const document = await Document.findById(req.params.id);
  if (document.userId !== req.user.id && !req.user.isAdmin) {
    return res.status(403).json({ error: 'Forbidden' });
  }
  next();
};

// ❌ NEVER trust client-side checks alone
// Client: if (isAdmin) { showAdminPanel(); }
// This can be bypassed! Always verify server-side.
```

### Multi-Factor Authentication (MFA)

- Implement TOTP (Time-based One-Time Password)
- Use libraries like `speakeasy` or `otplib`
- Provide backup codes for account recovery
- Store MFA secrets encrypted
- Support authenticator apps (Google Authenticator, Authy)

---

## Input Validation & Sanitization

### SQL Injection Prevention

❌ **NEVER:**
```javascript
// Raw SQL with string interpolation
const query = `SELECT * FROM users WHERE email = '${email}'`;
db.query(query);

// This allows: email = "' OR '1'='1"
```

✅ **ALWAYS:**
```javascript
// Use parameterized queries
const query = 'SELECT * FROM users WHERE email = $1';
db.query(query, [email]);

// Or use ORM with proper escaping
const user = await User.findOne({ where: { email } });
```

### NoSQL Injection Prevention

❌ **NEVER:**
```javascript
// Dangerous in MongoDB
User.findOne({ username: req.body.username });
// Attack: { username: { $gt: "" } } returns all users
```

✅ **ALWAYS:**
```javascript
// Validate and sanitize input
const username = String(req.body.username);
User.findOne({ username });

// Or use schema validation
const { error, value } = userSchema.validate(req.body);
if (error) return res.status(400).json({ error: error.details });
```

### XSS (Cross-Site Scripting) Prevention

❌ **NEVER:**
```javascript
// Dangerous HTML insertion
element.innerHTML = userInput;
res.send(`<h1>Welcome ${userName}</h1>`);
```

✅ **ALWAYS:**
```javascript
// Escape HTML
const escape = require('escape-html');
res.send(`<h1>Welcome ${escape(userName)}</h1>`);

// Or use templating engines with auto-escaping
// React, Vue, Angular do this by default
<h1>Welcome {userName}</h1> // React escapes by default

// For rich text, use sanitizers
const sanitizeHtml = require('sanitize-html');
const clean = sanitizeHtml(userInput, {
  allowedTags: ['b', 'i', 'em', 'strong'],
  allowedAttributes: {}
});
```

### Input Validation

```javascript
// ✅ Validate all inputs
const Joi = require('joi');

const userSchema = Joi.object({
  email: Joi.string().email().required(),
  age: Joi.number().integer().min(0).max(120),
  username: Joi.string().alphanum().min(3).max(30).required(),
  password: Joi.string().min(12).required()
});

const { error, value } = userSchema.validate(req.body);
if (error) {
  return res.status(400).json({ error: error.details[0].message });
}
```

### File Upload Security

```javascript
// ✅ Validate file uploads
const multer = require('multer');

const upload = multer({
  limits: {
    fileSize: 5 * 1024 * 1024 // 5MB
  },
  fileFilter: (req, file, cb) => {
    // Whitelist allowed types
    const allowedTypes = ['image/jpeg', 'image/png', 'image/gif'];
    if (!allowedTypes.includes(file.mimetype)) {
      return cb(new Error('Invalid file type'));
    }
    cb(null, true);
  }
});

// ⚠️ Don't trust file extensions alone
// ⚠️ Scan files for malware if accepting from users
// ⚠️ Store uploads outside web root
// ⚠️ Use random filenames to prevent overwrites
```

---

## Secrets Management

### Never Commit Secrets

❌ **NEVER in code:**
```javascript
const API_KEY = 'sk_live_abc123xyz789'; // NEVER!
const dbPassword = 'password123'; // NEVER!
```

✅ **ALWAYS use environment variables:**
```javascript
const API_KEY = process.env.STRIPE_API_KEY;
const dbPassword = process.env.DB_PASSWORD;

// Validate on startup
if (!process.env.STRIPE_API_KEY) {
  throw new Error('STRIPE_API_KEY must be set');
}
```

### Environment Variables

**.env (local development only):**
```bash
# ⚠️ Add .env to .gitignore!
DATABASE_URL=postgresql://localhost:5432/mydb
JWT_SECRET=your-secret-key-here
STRIPE_API_KEY=sk_test_xyz
```

**.env.example (committed to repo):**
```bash
# Template for required environment variables
DATABASE_URL=
JWT_SECRET=
STRIPE_API_KEY=
```

### Production Secrets

Use a secrets management service:
- **AWS**: AWS Secrets Manager, Parameter Store
- **GCP**: Secret Manager
- **Azure**: Key Vault
- **HashiCorp**: Vault
- **Kubernetes**: Secrets

```javascript
// ✅ Load secrets from secrets manager
const AWS = require('aws-sdk');
const secretsManager = new AWS.SecretsManager();

async function getSecret(secretName) {
  const data = await secretsManager.getSecretValue({
    SecretId: secretName
  }).promise();
  return JSON.parse(data.SecretString);
}
```

### Secret Rotation

- Rotate secrets regularly (every 90 days)
- Automate rotation when possible
- Have a process for emergency rotation
- Log secret access for audit trails

---

## API Security

### Rate Limiting

```javascript
// ✅ Implement rate limiting
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  message: 'Too many requests, please try again later.',
  standardHeaders: true,
  legacyHeaders: false,
});

app.use('/api/', limiter);

// Stricter limits for sensitive endpoints
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5, // 5 attempts per 15 minutes
  skipSuccessfulRequests: true
});

app.post('/api/auth/login', authLimiter, loginHandler);
```

### CORS (Cross-Origin Resource Sharing)

```javascript
// ❌ Dangerous - allows all origins
app.use(cors({ origin: '*' }));

// ✅ Whitelist specific origins
const cors = require('cors');

const corsOptions = {
  origin: (origin, callback) => {
    const allowedOrigins = [
      'https://yourapp.com',
      'https://www.yourapp.com',
      process.env.NODE_ENV === 'development' ? 'http://localhost:3000' : null
    ].filter(Boolean);

    if (!origin || allowedOrigins.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'));
    }
  },
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization']
};

app.use(cors(corsOptions));
```

### HTTPS Only

```javascript
// ✅ Redirect HTTP to HTTPS
app.use((req, res, next) => {
  if (process.env.NODE_ENV === 'production' && !req.secure) {
    return res.redirect('https://' + req.headers.host + req.url);
  }
  next();
});

// ✅ Set security headers
const helmet = require('helmet');
app.use(helmet());

// ✅ Enforce HTTPS with HSTS
app.use(helmet.hsts({
  maxAge: 31536000, // 1 year
  includeSubDomains: true,
  preload: true
}));
```

### API Key Security

```javascript
// ✅ API keys in headers, not query params
// Query params get logged everywhere!

// ❌ Bad
GET /api/users?apiKey=secret123

// ✅ Good
GET /api/users
Authorization: Bearer secret123

// ✅ Validate API keys
const validateApiKey = async (req, res, next) => {
  const apiKey = req.headers['x-api-key'];

  if (!apiKey) {
    return res.status(401).json({ error: 'API key required' });
  }

  const hashedKey = hashApiKey(apiKey);
  const validKey = await ApiKey.findOne({ hash: hashedKey });

  if (!validKey || !validKey.isActive) {
    return res.status(403).json({ error: 'Invalid API key' });
  }

  req.apiKey = validKey;
  next();
};
```

---

## Database Security

### Connection Security

```javascript
// ✅ Use SSL/TLS for database connections
const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: {
    rejectUnauthorized: true,
    ca: fs.readFileSync('/path/to/ca-cert.pem').toString()
  }
});
```

### Principle of Least Privilege

```sql
-- ✅ Create application-specific user with minimal permissions
CREATE USER app_user WITH PASSWORD 'strong_password';

-- Only grant necessary permissions
GRANT SELECT, INSERT, UPDATE, DELETE ON users TO app_user;
GRANT SELECT, INSERT, UPDATE, DELETE ON orders TO app_user;

-- ❌ Don't use database root/admin user in application
-- ❌ Don't grant ALL PRIVILEGES unless absolutely necessary
```

### Encryption at Rest

- Enable database encryption (AWS RDS, GCP Cloud SQL support this)
- Encrypt sensitive columns (PII, financial data)
- Use deterministic encryption for searchable fields
- Use random encryption for non-searchable sensitive data

```javascript
// ✅ Encrypt sensitive data
const crypto = require('crypto');

const algorithm = 'aes-256-gcm';
const key = Buffer.from(process.env.ENCRYPTION_KEY, 'hex');

function encrypt(text) {
  const iv = crypto.randomBytes(16);
  const cipher = crypto.createCipheriv(algorithm, key, iv);

  let encrypted = cipher.update(text, 'utf8', 'hex');
  encrypted += cipher.final('hex');

  const authTag = cipher.getAuthTag();

  return {
    encrypted,
    iv: iv.toString('hex'),
    authTag: authTag.toString('hex')
  };
}
```

### Backup Security

- Encrypt backups
- Store backups in separate location
- Restrict access to backups
- Test restore procedures regularly
- Automate backup verification

---

## Dependency Security

### Regular Updates

```bash
# Check for vulnerabilities
npm audit

# Fix automatically (review changes!)
npm audit fix

# For breaking changes
npm audit fix --force
```

### Dependency Scanning

```yaml
# .github/workflows/security.yml
name: Security Scan

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 0 * * 0' # Weekly

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Run npm audit
        run: npm audit --audit-level=high

      - name: Run Snyk
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
```

### Dependency Review

Before adding dependencies:
- ✅ Check npm weekly downloads (popularity)
- ✅ Check last update date (maintained?)
- ✅ Review GitHub issues (security issues?)
- ✅ Check dependencies (does it pull in 100 packages?)
- ✅ Review license (compatible with your use?)
- ✅ Consider bundle size impact

### Lock Files

```bash
# ✅ Always commit lock files
git add package-lock.json
git add yarn.lock

# ✅ Use exact versions in production
npm ci  # Uses exact versions from lock file
```

---

## Logging & Monitoring

### Secure Logging

❌ **NEVER log:**
```javascript
// Passwords
logger.info('User login:', { email, password }); // NO!

// Tokens
logger.info('Request:', { headers: req.headers }); // Contains auth tokens!

// Credit cards
logger.info('Payment:', { cardNumber, cvv }); // NO!

// API keys
logger.error('API error:', { apiKey, error }); // NO!
```

✅ **DO log:**
```javascript
// Sanitized data
logger.info('User login attempt', {
  email,
  ip: req.ip,
  userAgent: req.get('user-agent'),
  success: true
});

// Redacted sensitive fields
const sanitized = {
  ...paymentData,
  cardNumber: maskCardNumber(paymentData.cardNumber),
  cvv: '***'
};
logger.info('Payment processed', sanitized);
```

### Security Events to Log

```javascript
// ✅ Authentication events
logger.info('auth.login.success', { userId, ip, timestamp });
logger.warn('auth.login.failed', { email, ip, reason, timestamp });
logger.warn('auth.login.brute_force', { ip, attempts, timestamp });

// ✅ Authorization failures
logger.warn('auth.forbidden', { userId, resource, action, timestamp });

// ✅ Data access
logger.info('data.access', { userId, resource, action, timestamp });
logger.warn('data.access.suspicious', { userId, resource, pattern });

// ✅ Configuration changes
logger.info('config.change', { userId, setting, oldValue: '***', newValue: '***' });

// ✅ Security events
logger.error('security.injection_attempt', { type: 'sql', input, ip });
logger.error('security.xss_attempt', { input, ip });
```

### Monitoring & Alerting

Set up alerts for:
- Unusual login patterns
- High rate of failed authentication
- Access to sensitive resources
- Changes to security settings
- Dependency vulnerabilities detected
- SSL certificate expiration (30 days before)

---

## Frontend Security

### Content Security Policy (CSP)

```javascript
// ✅ Set restrictive CSP headers
app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"],
    scriptSrc: ["'self'", "'unsafe-inline'"], // Avoid unsafe-inline in production
    styleSrc: ["'self'", "'unsafe-inline'"],
    imgSrc: ["'self'", 'data:', 'https:'],
    connectSrc: ["'self'", 'https://api.yourapp.com'],
    fontSrc: ["'self'"],
    objectSrc: ["'none'"],
    upgradeInsecureRequests: []
  }
}));
```

### Prevent Clickjacking

```javascript
// ✅ X-Frame-Options header
app.use(helmet.frameguard({ action: 'deny' }));
```

### CSRF Protection

```javascript
// ✅ Use CSRF tokens for state-changing operations
const csrf = require('csurf');
const csrfProtection = csrf({ cookie: true });

app.get('/form', csrfProtection, (req, res) => {
  res.render('form', { csrfToken: req.csrfToken() });
});

app.post('/process', csrfProtection, (req, res) => {
  // Process form
});
```

### Secure Cookie Settings

```javascript
// ✅ Secure cookie configuration
res.cookie('session', sessionId, {
  httpOnly: true,      // Prevents JavaScript access
  secure: true,        // HTTPS only
  sameSite: 'strict',  // CSRF protection
  maxAge: 3600000,     // 1 hour
  signed: true         // Signature to detect tampering
});
```

---

## Infrastructure Security

### Container Security

```dockerfile
# ✅ Use specific versions, not 'latest'
FROM node:18.15.0-alpine

# ✅ Run as non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nodejs -u 1001
USER nodejs

# ✅ Don't include development dependencies
RUN npm ci --only=production

# ✅ Scan images for vulnerabilities
# docker scan your-image:tag
```

### Network Security

```yaml
# ✅ Use security groups / firewall rules
# Only allow necessary inbound traffic
Inbound Rules:
  - Port 443 (HTTPS): 0.0.0.0/0
  - Port 22 (SSH): Your-Office-IP/32
  - Database: Only from application servers

# ✅ Use VPC / private networks
# Keep databases in private subnets
```

### Kubernetes Security

```yaml
# ✅ Security context
apiVersion: v1
kind: Pod
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
  containers:
    - name: app
      securityContext:
        allowPrivilegeEscalation: false
        readOnlyRootFilesystem: true
        capabilities:
          drop:
            - ALL
```

---

## Compliance & Data Protection

### GDPR / Data Privacy

- [ ] Implement right to data deletion
- [ ] Provide data export functionality
- [ ] Obtain explicit consent for data collection
- [ ] Have clear privacy policy
- [ ] Document data retention policies
- [ ] Implement data minimization (collect only what's needed)
- [ ] Encrypt PII (Personally Identifiable Information)

### PCI DSS (If handling payments)

- ✅ **Never** store CVV/CVC codes
- ✅ **Never** store full card numbers (use tokenization)
- ✅ Use PCI-compliant payment processors (Stripe, Square)
- ✅ Implement strong access controls
- ✅ Maintain audit logs

### SOC 2 Compliance

- Implement access controls
- Enable audit logging
- Encrypt data in transit and at rest
- Regular security testing
- Incident response procedures
- Vendor risk management

---

## Security Testing

### Automated Security Tests

```javascript
// ✅ Test authentication
describe('Authentication', () => {
  it('should reject requests without token', async () => {
    const res = await request(app).get('/api/protected');
    expect(res.status).toBe(401);
  });

  it('should reject invalid tokens', async () => {
    const res = await request(app)
      .get('/api/protected')
      .set('Authorization', 'Bearer invalid');
    expect(res.status).toBe(401);
  });
});

// ✅ Test authorization
describe('Authorization', () => {
  it('should prevent users from accessing others data', async () => {
    const res = await request(app)
      .get('/api/users/456')
      .set('Authorization', `Bearer ${userToken}`)
    expect(res.status).toBe(403);
  });
});

// ✅ Test input validation
describe('Input Validation', () => {
  it('should reject SQL injection attempts', async () => {
    const res = await request(app)
      .post('/api/login')
      .send({ email: "' OR '1'='1", password: "anything" });
    expect(res.status).toBe(400);
  });
});
```

### Penetration Testing

- Conduct annual penetration tests
- Test critical paths quarterly
- Use bug bounty programs
- Hire professional security auditors

### Security Checklist for PRs

- [ ] No secrets in code
- [ ] Input validation implemented
- [ ] Authentication/authorization checked
- [ ] SQL injection prevented (parameterized queries)
- [ ] XSS prevented (proper escaping)
- [ ] CSRF protection in place (for state changes)
- [ ] Rate limiting applied (for sensitive endpoints)
- [ ] Errors don't leak sensitive info
- [ ] Dependencies scanned for vulnerabilities
- [ ] Security tests added

---

## Incident Response

### Security Incident Procedure

1. **Detect & Report** - Immediately report to security team
2. **Contain** - Isolate affected systems
3. **Assess** - Determine scope and impact
4. **Eradicate** - Remove threat
5. **Recover** - Restore systems
6. **Learn** - Post-mortem and improvements

### Breach Notification

- Notify affected users within 72 hours (GDPR requirement)
- Notify authorities if required by law
- Provide clear information about what happened
- Explain what you're doing to prevent recurrence

---

## Security Resources

### Tools
- **SAST** (Static Analysis): SonarQube, Semgrep
- **DAST** (Dynamic Analysis): OWASP ZAP, Burp Suite
- **Dependency Scanning**: Snyk, Dependabot, npm audit
- **Secrets Scanning**: GitGuardian, TruffleHog
- **Container Scanning**: Trivy, Clair

### Learning
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [Node.js Security Best Practices](https://nodejs.org/en/docs/guides/security/)

### Internal
- Security team: security@datacode.app
- Security training: [Link to internal training]
- Report vulnerability: [Link to security disclosure process]

---

## Security Champions Program

Each team should have a Security Champion who:
- Stays up to date on security best practices
- Reviews PRs with security focus
- Advocates for security in design discussions
- Coordinates with central security team
- Leads security training for their team

---

**Remember: Security is everyone's responsibility. When in doubt, ask! It's better to ask a "dumb question" than to create a security vulnerability.**

**Last Updated**: [Date]
**Next Review**: Quarterly
