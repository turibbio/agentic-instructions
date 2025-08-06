# Documentation Generator

Genera documentazione completa e professionale per il codice, API e architettura del progetto.

## Obiettivi

- Creare documentazione tecnica chiara e completa
- Generare API documentation automatica
- Documentare architettura e design decisions
- Creare guide per sviluppatori
- Mantenere documentazione aggiornata

## Istruzioni

1. **Tipi di Documentazione**:
   - **API Documentation**: Endpoints, parametri, responses
   - **Code Documentation**: Commenti inline, JSDoc/DocStrings
   - **Architecture Documentation**: Design patterns, data flow
   - **User Guides**: Come utilizzare il software
   - **Developer Guides**: Setup, contribuzioni, best practices
   - **README**: Overview del progetto

2. **Analisi del Codice**:
   - Estrai funzioni/classi pubbliche
   - Identifica parametri e return types
   - Rileva dependencies e integrations
   - Analizza configurazioni e environment
   - Documenta database schema se presente

3. **Struttura Documentazione**:
   - Overview e scopo del progetto
   - Installation e setup instructions  
   - Usage examples con codice
   - API reference completa
   - Architecture overview
   - Contributing guidelines
   - Troubleshooting common issues

## Documentation Templates

```markdown
## 📚 [Project Name] Documentation

### 🎯 Overview
[Descrizione del progetto e suoi obiettivi]

### 🚀 Quick Start
```bash
# Installation
npm install

# Setup
npm run setup

# Start development
npm run dev
```

### 📖 API Reference

#### [Endpoint/Function Name]
**Description**: [What it does]

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `param1` | `string` | Yes | [Description] |
| `param2` | `object` | No | [Description] |

**Example**:
```[linguaggio]
// Usage example
const result = await apiCall({
  param1: 'value',
  param2: { key: 'value' }
});
```

**Response**:
```json
{
  "status": "success",
  "data": {
    "result": "value"
  }
}
```

### 🏗️ Architecture

#### System Overview
[Diagramma o descrizione dell'architettura]

#### Data Flow
1. [Step 1]
2. [Step 2]
3. [Step 3]

#### Key Components
- **[Component]**: [Description and responsibility]
- **[Component]**: [Description and responsibility]

### 🛠️ Development

#### Setup Environment
```bash
# Commands for setup
```

#### Running Tests
```bash
# Test commands
```

#### Code Style
[Coding standards and conventions]

### 🔧 Configuration
| Variable | Default | Description |
|----------|---------|-------------|
| `ENV_VAR` | `default` | [Description] |

### 📝 Examples
[Practical usage examples]

### ❓ FAQ
**Q: [Common question]**  
A: [Answer]

### 🐛 Troubleshooting
- **Issue**: [Problem description]  
  **Solution**: [How to fix]

### 🤝 Contributing
[Guidelines for contributors]
```

Genera documentazione modulare che può essere facilmente mantenuta e aggiornata automaticamente.