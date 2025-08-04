# Instructions Template Repository

Questo repository contiene un template base per le istruzioni di sviluppo utilizzabili sia con **Claude** che con **GitHub Copilot**. Fornisce una struttura organizzata di best practices, linee guida e convenzioni per progetti software.

## 📁 Struttura del Repository

### `.claude/` - Istruzioni per Claude
Directory contenente le istruzioni specifiche per Claude Code (claude.ai/code):

- **`instructions/`** - Istruzioni di sviluppo organizzate per dominio
- **`context/`** - File di contesto per progetti specifici
- **`templates/`** - Template riutilizzabili per nuovi progetti

### `.github/` - Istruzioni per GitHub Copilot
Directory contenente le istruzioni per GitHub Copilot:

- **`instructions/`** - Linee guida e best practices per lo sviluppo

### `CLAUDE.template.md`
Template base per il file `CLAUDE.md` da utilizzare nei progetti. Contiene:
- Panoramica del progetto
- Comandi di sviluppo
- Architettura e struttura
- Riferimenti alle istruzioni dettagliate

## 📋 Istruzioni Disponibili

### Backend & API
- **`aspnet-rest-apis.instructions.md`** - Linee guida per API REST con ASP.NET
- **`backend.instructions.md`** - Best practices generali per il backend
- **`csharp.instructions.md`** - Convenzioni e standard per C#
- **`dotnet-architecture-good-practices.instructions.md`** - Principi DDD e architettura .NET

### Frontend
- **`frontend.instructions.md`** - Linee guida generali per lo sviluppo frontend
- **`reactjs.instructions.md`** - Standard e best practices per ReactJS

### DevOps & Deployment
- **`devops-core-principles.instructions.md`** - Principi fondamentali DevOps (CALMS, DORA metrics)
- **`github-actions-ci-cd-best-practices.instructions.md`** - CI/CD con GitHub Actions
- **`containerization-docker-best-practices.instructions.md`** - Best practices Docker
- **`deployment.instructions.md`** - Strategie di deployment

### Performance & Monitoring
- **`performance-optimization.instructions.md`** - Ottimizzazione delle performance
- **`caching-performance.instructions.md`** - Strategie di caching
- **`monitoring-observability.instructions.md`** - Monitoring e osservabilità

### Security & Quality
- **`security-and-owasp.instructions.md`** - Sicurezza e linee guida OWASP
- **`testing.instructions.md`** - Strategie e best practices di testing
- **`validation-error-handling.instructions.md`** - Gestione errori e validazione

### Database & Data
- **`database-design.instructions.md`** - Design e architettura database

### Workflow & Documentation
- **`spec-driven-workflow-v1.instructions.md`** - Workflow guidato dalle specifiche
- **`memory-bank.instructions.md`** - Gestione della memoria progettuale
- **`copilot-thought-logging.instructions.md`** - Logging del processo di sviluppo
- **`conventional-commit.prompt.md`** - Convenzioni per i commit
- **`self-explanatory-code-commenting.instructions.md`** - Linee guida per i commenti
- **`taming-copilot.instructions.md`** - Controllo e gestione di Copilot

### Code Quality
- **`object-calisthenics.instructions.md`** - Principi di Object Calisthenics
- **`general.instructions.md`** - Linee guida generali di sviluppo

### Tools & Integration
- **`playwright-typescript.instructions.md`** - Testing con Playwright e TypeScript
- **`localization.instructions.md`** - Localizzazione e internazionalizzazione
- **`markdown.instructions.md`** - Standard per la documentazione Markdown

### Domain-Specific
- **`domain.instructions.md`** - Istruzioni specifiche per il dominio del progetto
- **`api-versioning.instructions.md`** - Gestione del versioning delle API

## 🚀 Come Utilizzare

### Per un Nuovo Progetto

1. **Copia il template**: Utilizza `CLAUDE.template.md` come base per il tuo `CLAUDE.md`
2. **Seleziona le istruzioni**: Copia dalla cartella `.claude/instructions/` o `.github/instructions/` le istruzioni pertinenti al tuo progetto
3. **Personalizza**: Adatta i template e le istruzioni alle specifiche del tuo progetto
4. **Configura**: Aggiorna i riferimenti nei file di configurazione

### File con Template (.template.md)

Molte istruzioni hanno un file `.template.md` corrispondente che fornisce:
- Struttura base personalizzabile
- Placeholder per informazioni specifiche del progetto
- Esempi e sezioni opzionali

### Best Practices

- **Mantieni la coerenza**: Usa lo stesso set di istruzioni per tutto il team
- **Aggiorna regolarmente**: Le best practices evolvono, mantieni le istruzioni aggiornate
- **Personalizza intelligentemente**: Adatta le istruzioni al contesto specifico senza perdere i principi fondamentali
- **Documenta le deviazioni**: Se ti discosti dalle istruzioni standard, documenta il perché

## 📚 Documentazione Aggiuntiva

- **`usage-guide.md`** - Guida dettagliata all'utilizzo delle istruzioni
- **`README.template.md`** - Template per README di progetti

## 🤝 Contribuire

Per aggiungere nuove istruzioni o migliorare quelle esistenti:

1. Segui la struttura esistente
2. Includi esempi pratici
3. Mantieni la coerenza con lo stile
4. Aggiungi sia la versione finale che il template se applicabile

## 📄 Licenza

Questo progetto è rilasciato sotto [LICENSE](LICENSE).