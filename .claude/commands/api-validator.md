# API Validator

Valida, testa e analizza API REST/GraphQL per conformità, performance e sicurezza.

**Uso**: `@api-validator [ENDPOINT|SPEC_FILE] [--type TYPE] [--test TEST_TYPE]`

**Esempi**:
- `@api-validator https://api.example.com` (analisi completa)
- `@api-validator swagger.json --type openapi`
- `@api-validator /api/users --test security`
- `@api-validator . --type graphql --test performance`

**API Types**:
- `rest` - REST API (default)
- `graphql` - GraphQL API
- `openapi` - OpenAPI/Swagger specification
- `postman` - Postman collection

**Test Types**:
- `all` - Tutti i test (default)
- `schema` - Validazione schema/contratto
- `security` - Security testing
- `performance` - Performance e load testing
- `compatibility` - Backward compatibility

## Obiettivi

- Validare conformità a standard REST/GraphQL
- Testare sicurezza endpoints (OWASP API Top 10)
- Verificare performance e scalabilità
- Controllare backward compatibility
- Generare documentazione automatica

## Istruzioni

1. **Rilevamento automatico**:
   - Identifica tipo API (REST/GraphQL)
   - Rileva specification files (swagger.json, schema.graphql)
   - Scansiona endpoints disponibili
   - Determina authentication requirements

2. **Schema Validation**:
   - Verifica conformità OpenAPI 3.0/GraphQL spec
   - Valida request/response schemas
   - Controlla required fields e data types
   - Testa edge cases e boundary values
   - Verifica error response formats

3. **Security Testing (OWASP API Top 10)**:
   - **API1**: Broken Object Level Authorization
   - **API2**: Broken User Authentication  
   - **API3**: Excessive Data Exposure
   - **API4**: Lack of Resources & Rate Limiting
   - **API5**: Broken Function Level Authorization
   - **API6**: Mass Assignment
   - **API7**: Security Misconfiguration
   - **API8**: Injection
   - **API9**: Improper Assets Management
   - **API10**: Insufficient Logging & Monitoring

4. **Performance Testing**:
   - Response time analysis
   - Throughput testing
   - Concurrent user simulation
   - Memory/CPU usage monitoring
   - Database query optimization
   - Caching effectiveness

5. **Compatibility Testing**:
   - Schema evolution validation
   - Breaking changes detection
   - Version compatibility matrix
   - Deprecation warnings

## Validation Report Format

```markdown
## 🔍 API Validation Report

### 📋 API Overview
- **Endpoint**: https://api.example.com
- **Type**: REST API
- **Version**: v2.1.0
- **Endpoints Tested**: 47
- **Test Duration**: 3m 42s

### ✅ Schema Validation
| Endpoint | Method | Schema Valid | Issues |
|----------|--------|--------------|--------|
| /api/users | GET | ✅ | None |
| /api/users | POST | ⚠️ | Missing required field validation |
| /api/orders | GET | ❌ | Invalid response schema |

### 🔒 Security Analysis
| OWASP Category | Status | Critical Issues | Recommendations |
|----------------|--------|----------------|-----------------|
| Authentication | ✅ Good | 0 | Consider JWT refresh token |
| Authorization | ⚠️ Warning | 2 | Fix IDOR vulnerability |
| Data Exposure | ❌ Failed | 3 | Remove sensitive fields |
| Rate Limiting | ✅ Good | 0 | Current: 1000/hr per user |

### 🚨 Critical Security Issues
1. **Broken Object Level Authorization** - `/api/users/{id}`
   - Users can access other users' data by modifying ID
   - **Fix**: Implement proper authorization checks
   
2. **Excessive Data Exposure** - `/api/users/profile`
   - Response includes password hash and internal IDs
   - **Fix**: Use response DTOs to filter sensitive data

### ⚡ Performance Results
| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| Avg Response Time | 245ms | <200ms | ⚠️ |
| 95th Percentile | 850ms | <500ms | ❌ |
| Throughput | 1,200 req/s | 1,500 req/s | ⚠️ |
| Error Rate | 0.2% | <0.1% | ⚠️ |

### 📊 Performance Bottlenecks
1. **Database Queries**: 45% of response time
   - N+1 query problem in `/api/users` endpoint
   - Missing indexes on user_orders table
   
2. **External API Calls**: 30% of response time
   - Payment service timeout (5s → should be 1s)
   - No caching for user preferences

### 🔄 Compatibility Analysis
| Version | Breaking Changes | Deprecated | Migration Required |
|---------|------------------|------------|-------------------|
| v2.0 → v2.1 | 0 | 3 fields | No |
| v1.9 → v2.0 | 5 | 12 fields | Yes |

### 📋 OpenAPI Compliance
- ✅ Valid OpenAPI 3.0 specification
- ⚠️ Missing description for 12 endpoints
- ⚠️ No examples provided for request/response
- ❌ Inconsistent error response format

### 🧪 Test Coverage
```
Endpoints:     ████████████████████ 47/47 (100%)
Status Codes:  ████████████████▒▒▒▒ 245/305 (80%)
Error Cases:   ████████▒▒▒▒▒▒▒▒▒▒▒▒ 23/58 (40%)
```

### 🚀 Load Test Results
```
Concurrent Users: 100
Duration: 10 minutes
Total Requests: 72,000
Success Rate: 99.8%

Response Time Distribution:
< 100ms: ████████████ 60%
< 200ms: ████████ 25% 
< 500ms: ████ 12%
< 1s:    ▒▒ 2%
> 1s:    ▒ 1%
```

### 🔧 Recommended Actions

#### Immediate (High Priority)
1. **Fix IDOR vulnerability** in user endpoints
2. **Remove sensitive data** from API responses  
3. **Add rate limiting** to authentication endpoints
4. **Fix database N+1 queries**

#### This Sprint (Medium Priority)
1. Improve error response consistency
2. Add request/response examples to OpenAPI
3. Implement response caching for static data
4. Set up API monitoring dashboard

#### Next Month (Low Priority)
1. Implement GraphQL for flexible querying
2. Add API versioning strategy
3. Set up automated security scanning
4. Create developer documentation portal

### 🛠️ Test Commands Generated
```bash
# Security test with OWASP ZAP
zap-api-scan.py -t https://api.example.com/openapi.json

# Performance test with K6
k6 run --duration=10m --rps=100 api-load-test.js

# Schema validation with Spectral
spectral lint openapi.yaml --ruleset .spectral.yml
```
```

Includi sempre script automatizzati per i test e integrazione con CI/CD pipelines.