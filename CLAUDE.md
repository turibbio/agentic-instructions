# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

[PROJECT_NAME] is [PROJECT_DESCRIPTION]. The project uses [DOMAIN_LANGUAGE] terminology for the domain and UI.

**Tech Stack:**
- Backend: [BACKEND_TECH_STACK]
- Frontend: [FRONTEND_TECH_STACK]
- Testing: [TESTING_FRAMEWORK]
- Database: [DATABASE_SYSTEM]

## Development Commands

### Backend
```bash
cd backend
[BACKEND_RESTORE_COMMAND]                    # Restore dependencies
[BACKEND_BUILD_COMMAND]                      # Build the solution
[BACKEND_RUN_COMMAND]                        # Run API (http://localhost:[BACKEND_PORT])
[BACKEND_TEST_COMMAND]                       # Run all tests
[BACKEND_TEST_VERBOSE_COMMAND]               # Run tests with detailed output
```

[BACKEND_DOCS_URL] available at: `http://localhost:[BACKEND_PORT]/[DOCS_PATH]`

### Frontend
```bash
cd frontend
[FRONTEND_INSTALL_COMMAND]        # Install dependencies
[FRONTEND_DEV_COMMAND]            # Start dev server (http://localhost:[FRONTEND_PORT])
[FRONTEND_BUILD_COMMAND]          # Build for production
[FRONTEND_LINT_COMMAND]           # Run linter
[FRONTEND_PREVIEW_COMMAND]        # Preview production build
```

## Architecture & Structure

### Backend
- **Solution**: `backend/[SOLUTION_FILE]` - Main solution file
- **API**: `backend/src/[API_PROJECT]/` - Web API
- **Tests**: `backend/tests/[TEST_PROJECT]/` - Tests

### Frontend  
- **Structure**: Standard [FRONTEND_FRAMEWORK] app in `frontend/src/`
- **Tech**: [FRONTEND_TECH_DETAILS]

*Detailed patterns and conventions are available in `.claude/instructions/`*

## Naming Conventions

- **Backend**: [BACKEND_NAMING_CONVENTIONS]
- **Frontend**: [FRONTEND_NAMING_CONVENTIONS]
- **Domain Terms**: [DOMAIN_NAMING_STRATEGY]

*Complete naming guidelines in `.claude/instructions/general.instructions.md`*

## Domain Terminology

**Core Concepts:**
- **[DOMAIN_CONCEPT_1]**: [DESCRIPTION_1]
- **[DOMAIN_CONCEPT_2]**: [DESCRIPTION_2]
- **[DOMAIN_CONCEPT_3]**: [DESCRIPTION_3]
- **[DOMAIN_CONCEPT_4]**: [DESCRIPTION_4]
- **[DOMAIN_CONCEPT_5]**: [DESCRIPTION_5]

**[DOMAIN_CATEGORY_NAME]:**
- **[CATEGORY_ITEM_1]**: [CATEGORY_DESCRIPTION_1]
- **[CATEGORY_ITEM_2]**: [CATEGORY_DESCRIPTION_2]
- **[CATEGORY_ITEM_3]**: [CATEGORY_DESCRIPTION_3]

## Claude Code Instructions

This project includes specialized instructions and memory system for Claude Code:

### Instructions (`.claude/instructions/`)
- **Core**: general, backend, frontend, domain instructions
- **Specialized**: Additional files for testing, security, performance, etc.

### Memory Bank (`.claude/`)
- **instructions/memory-bank.instructions.md**: Project context, decisions, and established patterns
- **context/active-context.md**: Current work focus and recent changes
- **context/progress.md**: What works, what's left to build, known issues

These files ensure consistent code generation and maintain context between sessions.

## Quick Start

### Prerequisites
- **[PREREQUISITE_1]** ([VERSION_1] or later)
- **[PREREQUISITE_2]** ([VERSION_2] or later) + [PACKAGE_MANAGER]
- **[PREREQUISITE_3]** ([VERSION_3] or later)
- **Git** for version control

### 30-Second Setup
```bash
# Clone and setup
git clone <repository-url>
cd [PROJECT_FOLDER_NAME]

# Backend
cd backend && [BACKEND_RESTORE_COMMAND] && [BACKEND_BUILD_COMMAND]

# Frontend  
cd ../frontend && [FRONTEND_INSTALL_COMMAND]

# Run both (separate terminals)
cd backend && [BACKEND_RUN_COMMAND]      # :[BACKEND_PORT]
cd frontend && [FRONTEND_DEV_COMMAND]    # :[FRONTEND_PORT]
```

## Environment Variables

### Backend
```bash
# [BACKEND_CONFIG_FILE] or environment
[ENV_VAR_1]=[VALUE_1]
[ENV_VAR_2]="[CONNECTION_STRING_TEMPLATE]"
[ENV_VAR_3]="[SECRET_KEY_TEMPLATE]"
[ENV_VAR_4]="[ISSUER_TEMPLATE]"
[ENV_VAR_5]="[AUDIENCE_TEMPLATE]"
[ENV_VAR_6]="http://localhost:[FRONTEND_PORT]"
```

### Frontend
```bash
# .env.development
[FRONTEND_ENV_VAR_1]=http://localhost:[BACKEND_PORT]/api
[FRONTEND_ENV_VAR_2]="[PROJECT_NAME] Development"
[FRONTEND_ENV_VAR_3]=development
```

## Dependencies

### Backend Key Packages
```[CONFIG_FORMAT]
[BACKEND_DEPENDENCIES_TEMPLATE]
```

### Frontend Key Packages
```json
{
  "dependencies": {
    [FRONTEND_DEPENDENCIES_TEMPLATE]
  },
  "devDependencies": {
    [FRONTEND_DEV_DEPENDENCIES_TEMPLATE]
  }
}
```

## Testing & Development Guidelines

- **Backend**: [BACKEND_TEST_DETAILS]
- **Testing**: Follow [TESTING_PATTERN] pattern
- **Language**: [LANGUAGE_STRATEGY]
- **Code Style**: Self-documenting with descriptive names
- **Security**: Always validate input and handle errors properly

*Detailed guidelines in `.claude/instructions/testing.instructions.md` and other specialized files*

## Troubleshooting

### Common Issues

#### Backend Won't Start
```bash
# Check [BACKEND_RUNTIME] version
[VERSION_CHECK_COMMAND]  # Should be [MIN_VERSION]+

# Restore packages
cd backend && [BACKEND_RESTORE_COMMAND]

# Check port conflicts
netstat -tlnp | grep [BACKEND_PORT]
```

#### Frontend Build Errors
```bash
# Clear node_modules and reinstall
rm -rf node_modules package-lock.json
[FRONTEND_INSTALL_COMMAND]

# Check [FRONTEND_RUNTIME] version
[FRONTEND_VERSION_CHECK]  # Should be [FRONTEND_MIN_VERSION]+
```

#### Database Connection Issues
```bash
# Test [DATABASE_SYSTEM] connection
[DATABASE_TEST_CONNECTION_COMMAND]

# Check connection string in [CONFIG_FILE]
# Ensure database exists and user has permissions
```

#### [COMMON_ERROR_TYPE] Errors
- Verify [CONFIG_SETTING] includes [REQUIRED_VALUE]
- Check browser dev tools for exact error
- Ensure both frontend/backend are running

### Getting Help
- Check `.claude/context/progress.md` for known issues
- Review specific instruction files in `.claude/instructions/`
- Consult memory bank in `.claude/instructions/memory-bank.instructions.md`