# Code Quality Check

Esegue un'analisi completa della qualità del codice con metriche, linting, complexity e best practices.

**Uso**: `@code-quality-check [PATH] [--metric METRIC] [--threshold VALUE]`

**Esempi**:
- `@code-quality-check` (analisi completa del progetto)
- `@code-quality-check src/components/` (directory specifica)
- `@code-quality-check --metric complexity --threshold 10`
- `@code-quality-check src/utils.js --metric maintainability`

**Metriche disponibili**:
- `complexity` - Cyclomatic complexity
- `maintainability` - Maintainability Index
- `duplication` - Code duplication
- `coverage` - Test coverage
- `technical-debt` - Technical debt estimation
- `all` - Tutte le metriche (default)

## Obiettivi

- Misurare qualità del codice con metriche oggettive
- Identificare hotspot che necessitano refactoring
- Monitorare technical debt
- Verificare conformità a coding standards
- Suggerire miglioramenti specifici

## Istruzioni

1. **Rilevamento automatico**:
   - Identifica linguaggio e framework
   - Cerca configurazioni esistenti (ESLint, SonarQube, etc.)
   - Rileva tools di qualità già configurati
   - Determina standard di progetto

2. **Analisi Cyclomatic Complexity**:
   - Calcola complessità per funzione/metodo
   - Identifica funzioni con complessità > 10 (soglia modificabile)
   - Suggerisci refactoring per ridurre complessità
   - Mostra grafico distribuzione complessità

3. **Maintainability Index**:
   - Calcola MI basato su LOC, complessità, Halstead metrics
   - Classifica file per maintainability (A-F scale)
   - Identifica file con MI < 20 (difficili da mantenere)
   - Suggerisci strategie di miglioramento

4. **Code Duplication Analysis**:
   - Rileva blocchi di codice duplicato
   - Calcola percentuale di duplicazione
   - Identifica candidati per extract method/class
   - Suggerisci pattern per eliminare duplicazione

5. **Coding Standards**:
   - Verifica naming conventions
   - Controlla lunghezza funzioni/classi
   - Analizza nesting depth
   - Verifica presence di magic numbers

6. **Technical Debt**:
   - Calcola debt basato su code smells
   - Stima tempo per risolvere issues
   - Prioritizza debt per impatto
   - Suggerisci roadmap di miglioramento

## Quality Report Format

```markdown
## 📊 Code Quality Report

### 🎯 Overall Score: B+ (82/100)

### 📈 Key Metrics
| Metric | Score | Threshold | Status |
|--------|-------|-----------|---------|
| Complexity | 7.2 avg | < 10 | ✅ Good |
| Maintainability | 75 | > 60 | ✅ Good |
| Duplication | 8% | < 5% | ⚠️ Warning |
| Test Coverage | 85% | > 80% | ✅ Good |

### 🚨 Critical Issues
1. **High Complexity** (5 files):
   - `src/api/users.js:calculatePermissions()` - Complexity: 18
   - `src/utils/validators.js:validateForm()` - Complexity: 15

2. **Low Maintainability** (3 files):
   - `src/legacy/parser.js` - MI: 18 (F grade)
   - `src/helpers/format.js` - MI: 22 (D grade)

3. **Code Duplication** (12 blocks):
   - 45 lines duplicated in authentication logic
   - 23 lines duplicated in validation functions

### 📊 File Quality Distribution
```
A (90-100): ████████████ 35 files
B (80-89):  ████████     22 files  
C (70-79):  ████         12 files
D (60-69):  ██           6 files
F (0-59):   █            3 files
```

### 🔧 Recommended Actions
1. **Immediate (Technical Debt: 8h)**:
   - Refactor `calculatePermissions()` - split into smaller functions
   - Extract common validation logic to utility

2. **This Sprint (Technical Debt: 16h)**:
   - Rewrite `legacy/parser.js` using modern patterns
   - Add missing unit tests for coverage gaps

3. **Next Month (Technical Debt: 24h)**:
   - Implement consistent error handling pattern
   - Set up automated quality gates in CI/CD

### 💡 Quick Wins
- Add ESLint rule for max-complexity: 10
- Enable SonarQube quality gate
- Set up pre-commit hooks for linting
- Add complexity badge to README

### 📋 Quality Trend
```
Current:  ████████▒▒ 82%
Last week: ███████▒▒▒ 78%
Trend: ↗️ Improving (+4%)
```
```

Fornisci sempre comandi specifici per configurare quality tools e automatizzare i controlli.