# Claude Code Custom Commands

Questa cartella contiene un set di custom commands per Claude Code che estendono le funzionalità base con strumenti specializzati per lo sviluppo software.

## 🚀 Come Installare

1. **Posiziona i file**: I file `.md` in questa cartella sono già nel posto corretto (`/.claude/commands/`)
2. **Verifica configurazione**: Claude Code rileverà automaticamente i comandi al prossimo avvio
3. **Utilizzo**: Usa `@nome-comando` per invocare qualsiasi comando

## 📋 Comandi Disponibili

### 🔍 **API Validator** (`@api-validator`)
Valida e testa API REST/GraphQL per conformità, performance e sicurezza.

**Esempi**:
```bash
@api-validator https://api.example.com
@api-validator swagger.json --type openapi
@api-validator /api/users --test security
```

**Funzionalità**:
- Validazione schema OpenAPI/GraphQL
- Security testing (OWASP API Top 10)
- Performance e load testing
- Backward compatibility check
- Generazione report dettagliato

---

### 🌿 **Branch Management** (`@branch-management`)
Analizza e gestisce i branch del repository con suggerimenti intelligenti.

**Funzionalità**:
- Analisi stato branch esistenti
- Identificazione branch obsoleti
- Gestione merge e rebase conflicts
- Suggerimenti per cleanup
- Best practices per workflow

---

### ⚡ **Code Quality Check** (`@code-quality-check`)
Analisi completa della qualità del codice con metriche e best practices.

**Esempi**:
```bash
@code-quality-check
@code-quality-check src/components/
@code-quality-check --metric complexity --threshold 10
```

**Metriche**:
- Cyclomatic complexity
- Maintainability Index
- Code duplication
- Technical debt estimation
- Test coverage analysis

---

### 📝 **Create PR** (`@create-pr`)
Genera pull request complete con descrizioni dettagliate e checklist.

**Funzionalità**:
- Analisi automatica delle modifiche
- Generazione titolo e descrizione
- Identificazione reviewer appropriati
- Template PR standardizzato
- Integrazione con GitHub CLI

---

### 📦 **Dependency Analyzer** (`@dependency-analyzer`)
Analizza dipendenze per vulnerabilità, aggiornamenti e ottimizzazioni.

**Esempi**:
```bash
@dependency-analyzer
@dependency-analyzer --scope dev
@dependency-analyzer --check security
```

**Controlli**:
- Vulnerabilità di sicurezza
- Pacchetti obsoleti
- Dipendenze non utilizzate
- Duplicati e conflitti
- Analisi licenze

---

### 📚 **Documentation Generator** (`@documentation-generator`)
Genera documentazione tecnica completa e professionale.

**Tipi di documentazione**:
- API documentation automatica
- Code documentation (JSDoc/DocStrings)
- Architecture overview
- User e developer guides
- README e contributing guidelines

---

### 🔍 **Explain PR** (`@explain-pr`)
Analizza e spiega pull request esistenti in dettaglio.

**Funzionalità**:
- Analisi impatto modifiche
- Identificazione rischi potenziali
- Valutazione qualità codice
- Suggerimenti per review
- Assessment architetturale

---

### 📄 **File Review** (`@file-review`)
Review completa e approfondita di file di codice specifici.

**Aree di analisi**:
- Qualità e manutenibilità
- Security vulnerabilities
- Performance optimization
- Best practices compliance
- Suggerimenti refactoring

---

### ⚡ **Performance Optimizer** (`@performance-optimizer`)
Identifica bottleneck e suggerisce ottimizzazioni performance.

**Ottimizzazioni**:
- Algoritmi e complessità
- Memory management
- Database query optimization
- Caching strategies
- Async/parallel processing

---

### 🚀 **Push GitHub** (`@push-github`)
Push sicuro a GitHub con validazioni e controlli automatici.

**Controlli pre-push**:
- Security scan (credenziali, secrets)
- Quality gates (test, lint)
- Commit message validation
- Sync con remote
- Branch protection compliance

---

### 🔄 **Refactor** (`@refactor`)
Analizza codice e propone refactoring mirati per migliorare qualità.

**Opportunità identificate**:
- Extract Method/Class
- Code smell elimination
- SOLID principles compliance
- Design patterns application
- Complexity reduction

---

### 🔒 **Security Audit** (`@security-audit`)
Audit di sicurezza completo basato su OWASP Top 10.

**Controlli**:
- Injection vulnerabilities
- Authentication/Authorization flaws
- Sensitive data exposure
- Input validation issues
- Dependency vulnerabilities

---

### 🧪 **Test Generator** (`@test-generator`)
Genera test completi con scenari diversi ed edge cases.

**Tipi di test**:
- Unit tests con mocking
- Integration tests
- Edge cases e boundary values
- Error handling tests
- Performance tests

---

## 💡 Tips per l'Utilizzo

### Combinare Comandi
```bash
# Workflow completo di quality assurance
@code-quality-check src/
@security-audit src/
@test-generator src/components/Button.js
@performance-optimizer src/api/
```

### Personalizzazione
Ogni comando può essere personalizzato modificando il rispettivo file `.md` in questa cartella.

### Best Practices
1. **Pre-commit**: Usa `@code-quality-check` e `@security-audit` prima di ogni commit
2. **Pre-PR**: Combina `@create-pr` con `@file-review` per PR di qualità
3. **Manutenzione**: Esegui `@dependency-analyzer` settimanalmente
4. **Performance**: Usa `@performance-optimizer` durante code review

## 🛠️ Configurazione Avanzata

### Variables Globali
Alcuni comandi supportano variabili d'ambiente per personalizzazione:
- `CLAUDE_QUALITY_THRESHOLD`: Soglia qualità codice
- `CLAUDE_SECURITY_LEVEL`: Livello controlli security
- `CLAUDE_TEST_FRAMEWORK`: Framework di test preferito

### Integrazione CI/CD
I comandi possono essere integrati in pipeline automatiche per controlli continui.

---

## 🐛 Troubleshooting

**Comando non riconosciuto**: Verifica che il file `.md` sia in `/.claude/commands/`
**Errori di sintassi**: Controlla il formato markdown del comando
**Performance lenta**: I comandi complessi possono richiedere tempo extra

---

*Questi custom commands sono progettati per migliorare la produttività e la qualità del codice. Contributi e miglioramenti sono benvenuti!*