# File Review

Esegui una review completa e approfondita di uno o più file di codice.

## Obiettivi

- Analizzare la qualità del codice
- Identificare bug potenziali e vulnerabilità
- Suggerire miglioramenti architetturali
- Verificare conformità a best practices
- Proporre ottimizzazioni

## Istruzioni

1. Analizza ogni file fornito considerando:
   - **Struttura e organizzazione**
   - **Leggibilità e manutenibilità** 
   - **Performance e ottimizzazioni**
   - **Sicurezza e vulnerabilità**
   - **Testing e testabilità**
   - **Conformità agli standard**

2. Per ogni file, fornisci:
   - Riassunto generale
   - Punti di forza
   - Aree di miglioramento
   - Suggerimenti specifici con esempi di codice
   - Priorità delle modifiche (alta/media/bassa)

3. Aspetti specifici da verificare:
   - Gestione degli errori
   - Validazione input
   - Logging appropriato
   - Naming conventions
   - Commenti e documentazione
   - Dipendenze e imports
   - Resource management (memory leaks, connections)
   - Async/await patterns (se applicabile)

4. Suggerisci:
   - Refactoring opportuni
   - Pattern di design applicabili
   - Librerie o tools utili
   - Test cases mancanti

## Output Format

```markdown
## 📁 [Nome File]

### 🟢 Punti di Forza
- [Punto 1]
- [Punto 2]

### 🔴 Problemi Critici
- [Problema 1] - **ALTA PRIORITÀ**
- [Problema 2] - **MEDIA PRIORITÀ**

### 💡 Suggerimenti di Miglioramento
- [Suggerimento 1]
- [Suggerimento 2]

### 🔧 Codice Suggerito
```[linguaggio]
// Esempio di miglioramento
```
```

Concentrati su feedback actionable e specifico, evitando commenti generici.