# Claude Code Instructions

Questa cartella contiene le istruzioni specializzate per Claude Code per lo sviluppo di progetti software.

## File Principali

### Core Instructions
- **general.instructions.md**: Convenzioni generali del progetto e terminologia del dominio
- **backend.instructions.md**: Pattern e pratiche specifiche per .NET 8
- **frontend.instructions.md**: Pattern per React v19 + TypeScript
- **domain.instructions.md**: Logica di business e terminologia del dominio

### Instructions Specializzate

#### Development & Code Quality
- **testing.instructions.md**: Strategie e pattern di testing
- **self-explanatory-code-commenting.instructions.md**: Codice auto-documentante e commenti
- **object-calisthenics.instructions.md**: Best practices di coding e clean code
- **dotnet-architecture-good-practices.instructions.md**: Architettura .NET avanzata

#### Security & Performance  
- **security-and-owasp.instructions.md**: Sicurezza e best practices OWASP
- **caching-performance.instructions.md**: Ottimizzazione delle performance e caching
- **performance-optimization.instructions.md**: Ottimizzazioni generali delle performance
- **validation-error-handling.instructions.md**: Gestione di validazione ed errori

#### Technology Specific
- **aspnet-rest-apis.instructions.md**: Best practices per API REST in ASP.NET
- **csharp.instructions.md**: Convenzioni specifiche per C#
- **reactjs.instructions.md**: Pattern avanzati per React
- **playwright-typescript.instructions.md**: Testing E2E con Playwright

#### Operations & DevOps
- **monitoring-observability.instructions.md**: Monitoring e observability
- **containerization-docker-best-practices.instructions.md**: Docker e containerizzazione
- **conventional-commit.prompt.md**: Standard per commit messages
- **localization.instructions.md**: Internazionalizzazione e localizzazione

## Utilizzo

Questi file vengono utilizzati automaticamente da Claude Code per:
1. Mantenere coerenza nel codice generato
2. Seguire le convenzioni del progetto
3. Utilizzare la terminologia corretta per il dominio specifico
4. Applicare best practices specifiche per lo stack tecnologico

## Note

Le istruzioni sono sincronizzate con quelle presenti in `.github/instructions/` ma organizzate specificamente per l'utilizzo con Claude Code.