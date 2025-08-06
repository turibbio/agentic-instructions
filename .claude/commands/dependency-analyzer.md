# Dependency Analyzer

Analizza le dipendenze del progetto per identificare vulnerabilità, aggiornamenti, duplicati e ottimizzazioni.

**Uso**: `@dependency-analyzer [--scope SCOPE] [--check CHECK_TYPE]`

**Esempi**:
- `@dependency-analyzer` (analisi completa)
- `@dependency-analyzer --scope dev` (solo dev dependencies)
- `@dependency-analyzer --check security` (solo vulnerabilità)
- `@dependency-analyzer --check outdated` (solo pacchetti obsoleti)

**Scope disponibili**:
- `prod` - Solo production dependencies
- `dev` - Solo development dependencies
- `peer` - Peer dependencies
- `optional` - Optional dependencies

**Check types**:
- `security` - Vulnerabilità di sicurezza
- `outdated` - Pacchetti obsoleti
- `unused` - Dipendenze non utilizzate
- `duplicates` - Dipendenze duplicate
- `licenses` - Analisi licenze

## Obiettivi

- Identificare vulnerabilità di sicurezza nelle dipendenze
- Trovare pacchetti obsoleti e suggerire aggiornamenti
- Rilevare dipendenze non utilizzate
- Identificare duplicati e conflitti
- Analizzare licenze per compliance
- Ottimizzare bundle size

## Istruzioni

1. **Rilevamento automatico**:
   - Identifica package manager (npm, yarn, pnpm, pip, composer, etc.)
   - Legge file di configurazione (package.json, requirements.txt, etc.)
   - Rileva lockfiles per analisi precisa delle versioni

2. **Analisi Security**:
   - Scansiona database vulnerabilità (npm audit, Snyk, etc.)
   - Identifica CVE e CVSS scores
   - Suggerisci patch e workaround
   - Verifica dipendenze transitive

3. **Analisi Outdated**:
   - Confronta con ultime versioni disponibili
   - Identifica breaking changes potenziali
   - Suggerisci strategia di aggiornamento graduale
   - Verifica compatibilità tra dipendenze

4. **Analisi Usage**:
   - Scansiona codebase per import/require statements
   - Identifica dipendenze importate ma non utilizzate
   - Trova utilizzi indiretti tramite altri pacchetti
   - Suggerisci cleanup del package.json

5. **Bundle Analysis**:
   - Calcola impatto su bundle size
   - Identifica alternative più leggere
   - Suggerisci tree-shaking opportunities
   - Analizza import patterns

## Report Format

```markdown
## 📦 Dependency Analysis Report

### 🚨 Security Issues
| Package | Version | Vulnerability | Severity | Fix Available |
|---------|---------|---------------|----------|---------------|
| [pkg] | [ver] | [CVE-xxx] | HIGH/MED/LOW | [version] |

### 📈 Outdated Packages
| Package | Current | Latest | Type | Breaking Changes |
|---------|---------|--------|------|------------------|
| [pkg] | [cur] | [latest] | major/minor | Yes/No |

### 🗑️ Unused Dependencies
- **[package-name]** - Not imported anywhere
- **[package-name]** - Only used in commented code

### 🔄 Duplicates Found
- **lodash**: v4.17.21 (via express), v4.17.15 (direct)

### 💰 License Analysis
| Package | License | Compatibility | Risk |
|---------|---------|---------------|------|
| [pkg] | MIT | ✅ Compatible | Low |
| [pkg] | GPL-3.0 | ⚠️ Copyleft | High |

### 📊 Bundle Impact
- **Total size**: 2.3MB
- **Largest packages**: react (800KB), lodash (300KB)
- **Suggestions**: Use lodash-es for tree-shaking

### 🔧 Recommended Actions
1. **Immediate**: Fix 3 high-severity vulnerabilities
2. **This week**: Update 5 minor versions
3. **Next sprint**: Remove 8 unused dependencies
4. **Consider**: Replace moment.js with date-fns (-60KB)
```

Include sempre comandi specifici per il package manager rilevato per implementare le correzioni.