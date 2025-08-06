# Refactor

Analizza il codice esistente e propone refactoring mirati per migliorare qualità, performance e manutenibilità.

## Obiettivi

- Identificare code smells e anti-patterns
- Proporre refactoring sicuri e incrementali
- Migliorare architettura e design patterns
- Aumentare testabilità e manutenibilità
- Ottimizzare performance quando necessario

## Istruzioni

1. **Analisi del Codice Esistente**:
   - Identifica duplicazione di codice
   - Rileva funzioni/classi troppo complesse
   - Individua dipendenze problematiche
   - Verifica aderenza a principi SOLID
   - Analizza cyclomatic complexity

2. **Identificazione Opportunità**:
   - **Extract Method**: Funzioni troppo lunghe
   - **Extract Class**: Classi con troppe responsabilità  
   - **Move Method**: Metodi nella classe sbagliata
   - **Rename**: Nomi poco chiari
   - **Remove Dead Code**: Codice non utilizzato
   - **Introduce Parameter Object**: Troppi parametri
   - **Replace Magic Numbers**: Costanti hardcoded

3. **Prioritizzazione**:
   - **Alto impatto**: Miglioramenti architetturali significativi
   - **Medio impatto**: Cleanup e organizzazione
   - **Basso impatto**: Miglioramenti estetici

4. **Strategia di Refactoring**:
   - Proponi refactoring incrementali
   - Mantieni backward compatibility quando possibile
   - Suggerisci test per validare il refactoring
   - Identifica potenziali breaking changes

## Approccio Step-by-Step

```markdown
### 🎯 Obiettivo del Refactoring
[Descrizione del problema e obiettivo]

### 📋 Piano di Refactoring
1. **Step 1**: [Descrizione]
   - Rischi: [Eventuali rischi]
   - Validazione: [Come testare]

2. **Step 2**: [Descrizione]
   - Rischi: [Eventuali rischi]
   - Validazione: [Come testare]

### 💻 Codice Before/After
**Before:**
```[linguaggio]
// Codice originale
```

**After:**
```[linguaggio]
// Codice refactorizzato
```

### ✅ Benefici
- [Beneficio 1]
- [Beneficio 2]

### ⚠️ Considerazioni
- [Considerazione 1]
- [Considerazione 2]
```

Fornisci sempre esempi concreti e considera l'impatto sui test esistenti.