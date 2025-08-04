---
applyTo:
  - "**/*.cs"
  - "**/*.tsx"
  - "**/*.ts"
  - "**/*.json"
  - "backend/**/*"
  - "frontend/**/*"
---

# General Instructions for [PROJECT_NAME]

[PROJECT_NAME] is [PROJECT_DESCRIPTION] - provide a brief description of your project's purpose and target audience.

## Technology Stack

- **Backend**: [BACKEND_TECH] (e.g., .NET 8 Web API with Swagger/OpenAPI)
- **Frontend**: [FRONTEND_TECH] (e.g., React v19 + TypeScript + Vite)
- **Database**: [DATABASE_TECH] (e.g., PostgreSQL)
- **Language**: [UI_LANGUAGE] for user interface and documentation

## General Principles

### Naming Conventions

#### Backend ([BACKEND_LANGUAGE])
- **Classes and Interfaces**: PascalCase (`WeatherForecast`, `I[Service]Service`)
- **Methods**: PascalCase (`Get[Entity]`, `Create[Entity]`)
- **Properties**: PascalCase (`[Property]C`, `[Entity]Name`)
- **Local variables**: camelCase (`summaries`, `[entity]List`)
- **Constants**: PascalCase (`Max[Entity]s`)

#### Frontend (TypeScript/React)
- **React Components**: PascalCase (`[Entity]Card`, `[Entity]Form`)
- **Component files**: PascalCase with `.tsx` extension (`[Entity]Card.tsx`)
- **Custom hooks**: camelCase with `use` prefix (`use[Entity]Data`, `use[Action]`)
- **Variables and functions**: camelCase (`[entity]Data`, `handle[Action]`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_[ENTITIES]`, `API_BASE_URL`)
- **TypeScript types**: PascalCase (`[Entity]Data`, `[Entity]Form`)

### File Structure

#### Backend
```
backend/
├── src/
│   └── [ProjectName].Api/
│       ├── Controllers/     # API Controllers (if needed)
│       ├── Models/          # Data models
│       ├── Services/        # Business logic
│       ├── Data/           # Data access
│       └── Program.cs      # Entry point
```

#### Frontend
```
frontend/src/
├── components/        # Reusable components
├── pages/            # Main pages
├── hooks/            # Custom hooks
├── types/            # TypeScript definitions
├── utils/            # Utility functions
├── services/         # API calls
└── styles/           # CSS styles
```

## Best Practices

### Code
- Always use strict TypeScript (strict mode)
- Prefer functional React components with hooks
- Implement proper error handling
- Write self-documenting code with descriptive names
- Comments in [COMMENT_LANGUAGE] when needed for complex logic

### API Design
- Follow RESTful conventions
- Use appropriate HTTP status codes
- Implement input validation
- Document APIs with Swagger/OpenAPI
- Handle CORS for frontend

### Security
- Always validate user input
- Implement authentication/authorization
- Don't expose sensitive information
- Use HTTPS in production

## Domain Terminology

### [DOMAIN_CONTEXT]
- **[Core Entity 1]**: [Description]
- **[Core Entity 2]**: [Description]
- **[Core Entity 3]**: [Description]
- **[Core Entity 4]**: [Description]
- **[Core Entity 5]**: [Description]
- **[Core Entity 6]**: [Description]
- **[Core Entity 7]**: [Description]

### [DOMAIN_SUBCATEGORY]
- **[Type 1]**: [Description]
- **[Type 2]**: [Description]
- **[Type 3]**: [Description]
- **[Type 4]**: [Description]
- **[Type 5]**: [Description]

## Anti-Patterns to Avoid

- Don't mix [PRIMARY_LANGUAGE] and [SECONDARY_LANGUAGE] in variable/function names
- Don't use `any` in TypeScript without justification
- Avoid overly large React components (> 200 lines)
- Don't make API calls directly in components (use hooks/services)
- Avoid business logic in frontend
- Don't hardcode configuration values
- Don't use `var` in JavaScript/TypeScript
- Avoid direct React state mutations
- Don't forget error handling in API calls

## Common Development Commands

### Backend
```bash
cd backend
dotnet restore && dotnet build    # Initial setup
dotnet run --project src/[ProjectName].Api  # Start API (localhost:[PORT])
dotnet test                       # Run tests
```

### Frontend
```bash
cd frontend
npm install && npm run dev        # Setup and start (localhost:[PORT])
npm run build                     # Production build
npm run lint                      # Code check
```