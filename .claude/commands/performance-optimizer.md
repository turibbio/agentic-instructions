# Performance Optimizer

Analizza le performance del codice e suggerisce ottimizzazioni mirate per migliorare velocità e utilizzo delle risorse.

## Obiettivi

- Identificare colli di bottiglia nelle performance
- Suggerire ottimizzazioni specifiche per il linguaggio/framework
- Migliorare efficienza algoritmica
- Ridurre memory usage e garbage collection
- Ottimizzare operazioni I/O e database

## Istruzioni

1. **Analisi Performance**:
   - Identifica operazioni costose (O(n²), O(n³))
   - Rileva memory leaks potenziali
   - Analizza uso di CPU e memoria
   - Verifica operazioni I/O bloccanti
   - Controlla query database inefficienti

2. **Aree di Ottimizzazione**:
   - **Algoritmi**: Complessità temporale e spaziale
   - **Data Structures**: Scelta strutture dati appropriate
   - **Caching**: Implementazione cache intelligenti
   - **Lazy Loading**: Caricamento dati on-demand
   - **Batching**: Raggruppamento operazioni
   - **Async/Parallel**: Operazioni concorrenti
   - **Memory Management**: Garbage collection optimization

3. **Metriche da Considerare**:
   - Response time
   - Throughput
   - Memory usage
   - CPU utilization
   - Database query time
   - Network latency

## Performance Report Format

```markdown
## ⚡ Performance Analysis Report

### 🐌 Bottlenecks Identificati
| Issue | Location | Current Performance | Impact | Priority |
|-------|----------|-------------------|---------|----------|
| [Issue] | [File:Function] | [Current metric] | [Impact] | HIGH/MED/LOW |

### 📊 Benchmark Attuali
- **Response Time**: [X ms]
- **Memory Usage**: [X MB]
- **CPU Usage**: [X%]
- **Query Time**: [X ms]

### 🚀 Ottimizzazioni Proposte

#### 1. [Ottimizzazione Name]
**Problem**: [Descrizione problema]
**Solution**: [Soluzione proposta]
**Expected Improvement**: [Miglioramento atteso]

**Before**:
```[linguaggio]
// Codice non ottimizzato
```

**After**:
```[linguaggio]  
// Codice ottimizzato
```

**Benchmark Impact**: [Miglioramento previsto]

### 🛠️ Quick Wins
- [Quick win 1]: [Impatto stimato]
- [Quick win 2]: [Impatto stimato]

### 📈 Performance Monitoring
- [Tool/Metrica 1]: [Come monitorare]
- [Tool/Metrica 2]: [Come monitorare]

### 🔍 Profiling Suggestions
```bash
# Comandi per profiling
[comandi specifici per il linguaggio/framework]
```
```

Focus su ottimizzazioni con alto impatto e facile implementazione, sempre con dati quantitativi quando possibile.