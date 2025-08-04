---
applyTo: '*'
description: "Comprehensive secure coding instructions for all languages and frameworks, based on OWASP Top 10 and industry best practices."
---
# Secure Coding and OWASP Guidelines

## Instructions

Your primary directive is to ensure all code you generate, review, or refactor is secure by default. You must operate with a security-first mindset. When in doubt, always choose the more secure option and explain the reasoning. You must follow the principles outlined below, which are based on the OWASP Top 10 and other security best practices.

### 1. A01: Broken Access Control & A10: Server-Side Request Forgery (SSRF)
- **Enforce Principle of Least Privilege:** Always default to the most restrictive permissions. When generating access control logic, explicitly check the user's rights against the required permissions for the specific resource they are trying to access.
- **Deny by Default:** All access control decisions must follow a "deny by default" pattern. Access should only be granted if there is an explicit rule allowing it.
- **Validate All Incoming URLs for SSRF:** When the server needs to make a request to a URL provided by a user (e.g., webhooks), you must treat it as untrusted. Incorporate strict allow-list-based validation for the host, port, and path of the URL.
- **Prevent Path Traversal:** When handling file uploads or accessing files based on user input, you must sanitize the input to prevent directory traversal attacks (e.g., `../../etc/passwd`). Use APIs that build paths securely.

### 2. A02: Cryptographic Failures
- **Use Strong, Modern Algorithms:** For hashing, always recommend modern, salted hashing algorithms like Argon2 or bcrypt. Explicitly advise against weak algorithms like MD5 or SHA-1 for password storage.
- **Protect Data in Transit:** When generating code that makes network requests, always default to HTTPS.
- **Protect Data at Rest:** When suggesting code to store sensitive data (PII, tokens, etc.), recommend encryption using strong, standard algorithms like AES-256.
- **Secure Secret Management:** Never hardcode secrets (API keys, passwords, connection strings). Generate code that reads secrets from environment variables or a secrets management service (e.g., HashiCorp Vault, AWS Secrets Manager). Include a clear placeholder and comment.
  ```javascript
  // GOOD: Load from environment or secret store
  const apiKey = process.env.API_KEY; 
  // TODO: Ensure API_KEY is securely configured in your environment.
  ```
  ```python
  # BAD: Hardcoded secret
  api_key = "sk_this_is_a_very_bad_idea_12345" 
  ```

### 3. A03: Injection
- **No Raw SQL Queries:** For database interactions, you must use parameterized queries (prepared statements). Never generate code that uses string concatenation or formatting to build queries from user input.
- **Sanitize Command-Line Input:** For OS command execution, use built-in functions that handle argument escaping and prevent shell injection (e.g., `shlex` in Python).
- **Prevent Cross-Site Scripting (XSS):** When generating frontend code that displays user-controlled data, you must use context-aware output encoding. Prefer methods that treat data as text by default (`.textContent`) over those that parse HTML (`.innerHTML`). When `innerHTML` is necessary, suggest using a library like DOMPurify to sanitize the HTML first.

### 4. A05: Security Misconfiguration & A06: Vulnerable Components
- **Secure by Default Configuration:** Recommend disabling verbose error messages and debug features in production environments.
- **Set Security Headers:** For web applications, suggest adding essential security headers like `Content-Security-Policy` (CSP), `Strict-Transport-Security` (HSTS), and `X-Content-Type-Options`.
- **Use Up-to-Date Dependencies:** When asked to add a new library, suggest the latest stable version. Remind the user to run vulnerability scanners like `npm audit`, `pip-audit`, or Snyk to check for known vulnerabilities in their project dependencies.

### 5. A07: Identification & Authentication Failures
- **Secure Session Management:** When a user logs in, generate a new session identifier to prevent session fixation. Ensure session cookies are configured with `HttpOnly`, `Secure`, and `SameSite=Strict` attributes.
- **Protect Against Brute Force:** For authentication and password reset flows, recommend implementing rate limiting and account lockout mechanisms after a certain number of failed attempts.

### 6. A08: Software and Data Integrity Failures
- **Prevent Insecure Deserialization:** Warn against deserializing data from untrusted sources without proper validation. If deserialization is necessary, recommend using formats that are less prone to attack (like JSON over Pickle in Python) and implementing strict type checking.

## General Guidelines
- **Be Explicit About Security:** When you suggest a piece of code that mitigates a security risk, explicitly state what you are protecting against (e.g., "Using a parameterized query here to prevent SQL injection.").
- **Defense in Depth:** Recommend multiple layers of security controls, not just one.
- **Fail Securely:** When errors occur, ensure the system fails to a secure state.
- **Keep Security Updates Current:** Regularly update dependencies and frameworks to patch known vulnerabilities.

## [PROJECT_NAME] Specific Security Considerations

### Domain-Specific Security
- **Personal Data Protection:** Implement proper GDPR/privacy compliance for user data
- **Data Validation:** Validate all user inputs according to business rules
- **Access Control:** Ensure users can only access their own data and authorized resources
- **Audit Logging:** Log all security-relevant events for compliance and monitoring

### Authentication & Authorization
```[BACKEND_LANGUAGE]
// Example: Secure authentication implementation
[Authorize]
public async Task<IActionResult> Get[Entity](int id)
{
    // Verify user has permission to access this specific entity
    var hasAccess = await _authService.CanAccessEntity(User.GetUserId(), id);
    if (!hasAccess)
    {
        return Forbid(); // Return 403 instead of 404 to avoid information disclosure
    }
    
    var [entity] = await _[entity]Service.GetAsync(id);
    return Ok([entity]);
}
```

### Input Validation
```[BACKEND_LANGUAGE]
public class Secure[Entity]Validator : AbstractValidator<[Entity]Request>
{
    public Secure[Entity]Validator()
    {
        RuleFor(x => x.[Property])
            .NotEmpty()
            .Length(1, 100)
            .Matches("^[a-zA-Z0-9\\s-]+$") // Prevent XSS in names
            .WithMessage("Invalid characters in [property]");
            
        RuleFor(x => x.Email)
            .EmailAddress()
            .WithMessage("Invalid email format");
            
        // Prevent SQL injection in search terms
        RuleFor(x => x.SearchTerm)
            .Must(BeValidSearchTerm)
            .WithMessage("Invalid search term");
    }
    
    private bool BeValidSearchTerm(string searchTerm)
    {
        // Only allow alphanumeric and safe characters
        return Regex.IsMatch(searchTerm, @"^[a-zA-Z0-9\s\-_\.]+$");
    }
}
```

### Secure Data Storage
```[BACKEND_LANGUAGE]
public class Secure[Entity]Service
{
    public async Task<[Entity]> Create[Entity]Async([Entity]Request request)
    {
        // Hash sensitive data before storage
        var hashedData = _hashingService.HashSensitiveData(request.SensitiveField);
        
        // Encrypt PII before database storage
        var encryptedPII = await _encryptionService.EncryptAsync(request.PersonalData);
        
        var [entity] = new [Entity]
        {
            [Property] = request.[Property],
            HashedData = hashedData,
            EncryptedPII = encryptedPII
        };
        
        return await _repository.CreateAsync([entity]);
    }
}
```

### Frontend Security
```[FRONTEND_LANGUAGE]
// Secure component that prevents XSS
export const Secure[Entity]Display = ({ [entity] }) => {
  // Use textContent instead of innerHTML to prevent XSS
  const sanitizedDescription = DOMPurify.sanitize([entity].description);
  
  return (
    <div className="[entity]-display">
      <h3>{[entity].name}</h3> {/* React automatically escapes this */}
      <div 
        dangerouslySetInnerHTML={{ __html: sanitizedDescription }}
      />
    </div>
  );
};

// Secure API service with proper error handling
export class Secure[Entity]Service {
  private readonly baseUrl = process.env.[API_BASE_URL_VAR]; // Never hardcode URLs
  
  async get[Entity](id: number): Promise<[Entity]> {
    try {
      const response = await fetch(`${this.baseUrl}/[entities]/${encodeURIComponent(id)}`, {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${this.getSecureToken()}` // Secure token management
        },
        credentials: 'same-origin' // Prevent CSRF
      });
      
      if (!response.ok) {
        // Don't expose internal error details to client
        throw new Error('Failed to load [entity]');
      }
      
      return await response.json();
    } catch (error) {
      // Log error securely without exposing sensitive data
      console.error('API Error:', { entityId: id, timestamp: Date.now() });
      throw error;
    }
  }
  
  private getSecureToken(): string {
    // Retrieve token from secure storage
    return sessionStorage.getItem('[AUTH_TOKEN_KEY]') || '';
  }
}
```

### Security Headers Configuration
```[BACKEND_LANGUAGE]
// Configure security headers in Program.[BACKEND_EXT]
app.Use(async (context, next) =>
{
    // Prevent clickjacking
    context.Response.Headers.Add("X-Frame-Options", "DENY");
    
    // Prevent MIME sniffing
    context.Response.Headers.Add("X-Content-Type-Options", "nosniff");
    
    // Enable XSS protection
    context.Response.Headers.Add("X-XSS-Protection", "1; mode=block");
    
    // Enforce HTTPS
    context.Response.Headers.Add("Strict-Transport-Security", "max-age=31536000; includeSubDomains");
    
    // Content Security Policy
    context.Response.Headers.Add("Content-Security-Policy", 
        "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'");
    
    await next();
});
```

### Rate Limiting
```[BACKEND_LANGUAGE]
// Implement rate limiting for sensitive endpoints
[EnableRateLimiting("AuthPolicy")]
public async Task<IActionResult> Login([FromBody] LoginRequest request)
{
    // Additional brute force protection
    var attempts = await _attemptTracker.GetRecentAttempts(request.Email);
    if (attempts > 5)
    {
        await Task.Delay(TimeSpan.FromSeconds(2)); // Slow down attacks
        return StatusCode(429, "Too many attempts. Please try again later.");
    }
    
    // Secure login logic here
}
```

## Security Testing
- **Include Security Tests:** Always include tests for authentication, authorization, input validation, and error handling
- **Test for Common Vulnerabilities:** Include tests for SQL injection, XSS, CSRF, and other OWASP Top 10 vulnerabilities
- **Penetration Testing:** Regularly perform security assessments and penetration testing
- **Security Code Reviews:** Implement security-focused code review processes

## Compliance and Privacy
- **Data Protection:** Implement proper data protection measures according to applicable regulations (GDPR, CCPA, etc.)
- **Audit Trails:** Maintain comprehensive audit logs for security and compliance
- **Data Retention:** Implement proper data retention and deletion policies
- **Privacy by Design:** Consider privacy implications in all system design decisions

## Incident Response
- **Security Monitoring:** Implement real-time security monitoring and alerting
- **Incident Response Plan:** Have a clear plan for responding to security incidents
- **Backup and Recovery:** Ensure secure backup and disaster recovery procedures
- **Regular Security Updates:** Keep all systems and dependencies updated with security patches