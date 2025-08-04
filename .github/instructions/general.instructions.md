---
applyTo:
  - "**/*.[BACKEND_EXT]"
  - "**/*.[FRONTEND_EXT]"
  - "**/*.[CONFIG_EXT]"
  - "backend/**/*"
  - "frontend/**/*"
---

# Copilot Instructions for [PROJECT_NAME]

[PROJECT_NAME] is an open source platform for [PROJECT_DESCRIPTION] designed for [TARGET_AUDIENCE].

## Technologies Used

- **Backend**: [BACKEND_TECH] with [API_DOC_TOOL]
- **Frontend**: [FRONTEND_TECH]
- **Database**: [DATABASE_TECH] (to be implemented)
- **Language**: [UI_LANGUAGE] for user interface and documentation

## General Principles

### Naming Conventions

#### Backend ([BACKEND_TECH])
- **Classes and Interfaces**: PascalCase (`WeatherForecast`, `IEventService`)
- **Methods**: PascalCase (`GetWeatherForecast`, `CreateEvent`) 
- **Properties**: PascalCase (`TemperatureC`, `EventName`)
- **Local Variables**: camelCase (`summaries`, `eventList`)
- **Constants**: PascalCase (`MaxParticipants`)

#### Frontend ([FRONTEND_TECH])
- **Components**: PascalCase (`EventCard`, `RegistrationForm`)
- **Component Files**: PascalCase with extension `.[FRONTEND_EXT]` (`EventCard.[FRONTEND_EXT]`)
- **Custom Hooks**: camelCase with prefix `use` (`useEventData`, `useRegistration`)
- **Variables and Functions**: camelCase (`eventData`, `handleSubmit`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_PARTICIPANTS`, `API_BASE_URL`)
- **Types**: PascalCase (`EventData`, `RegistrationForm`)

### File Structure

#### Backend
```
backend/
├── src/
│   └── [PROJECT_NAME].Api/
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
- Always use strict TypeScript mode
- Prefer functional React components with hooks
- Implement proper error handling
- Write self-documenting code with descriptive names
- Comments in [COMMENT_LANGUAGE] when needed for complex logic

### API Design
- Follow RESTful conventions
- Use appropriate HTTP status codes
- Implement input validation
- Document APIs with [API_DOC_TOOL]
- Handle CORS for frontend

### Security
- Always validate user input
- Implement authentication/authorization
- Don't expose sensitive information
- Use HTTPS in production

## Domain Terminology

### [DOMAIN_CONTEXT]
- **[ENTITY_1]**: [Description of primary entity]
- **[ENTITY_2]**: [Description of secondary entity]
- **[ENTITY_3]**: [Description of third entity]
- **[ENTITY_4]**: [Description of fourth entity]
- **[ENTITY_5]**: [Description of results/output entity]
- **[ENTITY_6]**: [Description of tracking entity]
- **[ENTITY_7]**: [Description of identifier entity]

### [DOMAIN_SUBCATEGORY]
- **[SUBTYPE_1]**: [Description of first subtype]
- **[SUBTYPE_2]**: [Description of second subtype]
- **[SUBTYPE_3]**: [Description of third subtype]
- **[SUBTYPE_4]**: [Description of fourth subtype]
- **[SUBTYPE_5]**: [Description of fifth subtype]

## Anti-Patterns to Avoid

- Don't mix [UI_LANGUAGE] and [TECH_LANGUAGE] in variable/function names
- Don't use `any` in TypeScript without justification
- Avoid oversized React components (> 200 lines)
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
[RESTORE_COMMAND] && [BUILD_COMMAND]    # Initial setup
[RUN_COMMAND]                           # Start API (localhost:[BACKEND_PORT])
[TEST_COMMAND]                          # Run tests
```

### Frontend
```bash
cd frontend
[INSTALL_COMMAND] && [DEV_COMMAND]      # Setup and start (localhost:[FRONTEND_PORT])
[BUILD_COMMAND]                         # Production build
[LINT_COMMAND]                          # Code check
```