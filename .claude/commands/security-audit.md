# Security Audit

Esegui un audit di sicurezza completo del codice per identificare vulnerabilità e suggerire mitigazioni.

## Obiettivi

- Identificare vulnerabilità di sicurezza
- Analizzare gestione di autenticazione e autorizzazione
- Verificare protezione contro attacchi comuni
- Controllare gestione dei dati sensibili
- Suggerire best practices di security

## Istruzioni

1. **OWASP Top 10 Check**:
   - **Injection**: SQL, NoSQL, LDAP injection
   - **Broken Authentication**: Gestione sessioni, passwords
   - **Sensitive Data Exposure**: Encryption, storage sicuro
   - **XML External Entities (XXE)**: Parsing XML unsafe
   - **Broken Access Control**: Authorization flaws
   - **Security Misconfiguration**: Default configs, debug mode
   - **Cross-Site Scripting (XSS)**: Input sanitization
   - **Insecure Deserialization**: Unsafe object handling
   - **Known Vulnerabilities**: Dipendenze outdated
   - **Insufficient Logging**: Security event tracking

2. **Aree Specifiche da Analizzare**:
   - Input validation e sanitization
   - Output encoding
   - Authentication mechanisms
   - Session management
   - Crypto implementation
   - Error handling (information disclosure)
   - File upload security
   - API security
   - Database security
   - Third-party dependencies

3. **Code Analysis**:
   - Hardcoded secrets (API keys, passwords)
   - Unsafe functions usage
   - Privilege escalation risks
   - Race conditions
   - Buffer overflows (linguaggi applicabili)
   - Path traversal vulnerabilities

## Security Report Format

```markdown
## 🔒 Security Audit Report

### 🚨 Vulnerabilità Critiche
| Severity | Issue | Location | Impact | Fix Priority |
|----------|-------|----------|--------|--------------|
| HIGH     | [Issue] | [File:Line] | [Impact] | IMMEDIATE |

### 🟡 Vulnerabilità Medie
| Severity | Issue | Location | Recommendation |
|----------|-------|----------|----------------|
| MEDIUM   | [Issue] | [File:Line] | [Recommendation] |

### 💡 Raccomandazioni Generali
- [Raccomandazione 1]
- [Raccomandazione 2]

### 🔧 Fix Suggeriti
```[linguaggio]
// Codice vulnerabile
[codice problematico]

// Fix suggerito
[codice sicuro]
```

### 📚 Security Best Practices
- [Best practice 1]
- [Best practice 2]

### 🛠️ Tools Consigliati
- [Tool 1]: [Descrizione e uso]
- [Tool 2]: [Descrizione e uso]
```

Include sempre riferimenti a OWASP guidelines e security standards rilevanti.