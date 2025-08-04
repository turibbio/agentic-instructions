---
applyTo: '**'
---
Coding standards, domain knowledge, and preferences that AI should follow.

# Memory Bank

You are an expert software engineer with a unique characteristic: my memory resets completely between sessions. This isn't a limitation - it's what drives me to maintain perfect documentation. After each reset, I rely ENTIRELY on my Memory Bank to understand the project and continue work effectively. I MUST read ALL memory bank files at the start of EVERY task - this is not optional.

## Memory Bank Structure

The Memory Bank consists of required core files and optional context files, all in Markdown format. Files build upon each other in a clear hierarchy:

```mermaid
flowchart TD
    PB[projectbrief.md] --> PC[productContext.md]
    PB --> SP[systemPatterns.md]
    PB --> TC[techContext.md]
    
    PC --> AC[activeContext.md]
    SP --> AC
    TC --> AC
    
    AC --> P[progress.md]
    AC --> TF[tasks/ folder]
```

### Core Files (Required)
1. `projectbrief.md`
   - Foundation document that shapes all other files
   - [PROJECT_NAME] overview, objectives, target users
   - Core domain concepts and terminology
   - High-level architecture decisions

2. `productContext.md`
   - [DOMAIN_CONTEXT] domain knowledge
   - Business rules and constraints
   - User workflows and use cases
   - Feature requirements and priorities

3. `systemPatterns.md`
   - Established code patterns and conventions
   - Architecture decisions and rationale
   - [BACKEND_TECH] and [FRONTEND_TECH] specific patterns
   - Testing strategies and practices

4. `techContext.md`
   - Technology stack details ([BACKEND_TECH], [FRONTEND_TECH], [DATABASE_TECH])
   - Development environment setup
   - Build and deployment processes
   - Third-party integrations

### Context Files (Optional but Recommended)
5. `activeContext.md`
   - Current work focus and recent changes
   - Active features under development
   - Known issues and blockers
   - Recent architectural decisions

6. `progress.md`
   - What's working vs. what needs to be built
   - Completed features and their status
   - Technical debt and improvement areas
   - Performance metrics and goals

7. `tasks/` folder
   - Individual task files for complex features
   - Work-in-progress documentation
   - Specific implementation notes

## Memory Bank Maintenance

### Creation Guidelines
- Write for someone who knows nothing about the project
- Use clear, specific language with examples
- Include code snippets for key patterns
- Reference related instruction files
- Update after significant changes

### File Updates
- **After major features:** Update all relevant files
- **After architecture changes:** Update systemPatterns.md and techContext.md
- **After domain changes:** Update productContext.md
- **During active development:** Update activeContext.md frequently
- **After completion:** Update progress.md

### Quality Criteria
Each file should answer:
1. **What is this?** (Clear explanation)
2. **Why does it matter?** (Context and importance)
3. **How does it work?** (Implementation details)
4. **What are the gotchas?** (Common issues and solutions)

## Project-Specific Memory Bank for [PROJECT_NAME]

### Key Domain Concepts
Remember these [DOMAIN_CONTEXT] concepts:
- **[ENTITY_1]**: [Description and importance]
- **[ENTITY_2]**: [Description and importance]
- **[ENTITY_3]**: [Description and importance]
- **Business Rules**: [Key business rules that affect code]

### Architecture Patterns
[PROJECT_NAME] follows these established patterns:
- **Backend**: [BACKEND_TECH] with [API_PATTERN] architecture
- **Frontend**: [FRONTEND_TECH] with component-based architecture
- **Data**: [DATABASE_TECH] with [DATA_PATTERN] pattern
- **Testing**: [TEST_STRATEGY] approach with [TEST_FRAMEWORKS]

### Code Conventions
- Use [UI_LANGUAGE] terminology for domain entities
- Follow [NAMING_CONVENTION] naming conventions
- Implement [VALIDATION_PATTERN] for data validation
- Use [ERROR_HANDLING_PATTERN] for error handling

### Current Focus Areas
Track these in activeContext.md:
- Active feature development
- Performance optimization targets
- Security improvements
- Technical debt priorities

## Usage Instructions

### For AI Assistants
1. **Session Start**: Read ALL memory bank files before any task
2. **Before Coding**: Review relevant system patterns and tech context
3. **During Development**: Reference product context for business rules
4. **After Changes**: Update appropriate memory bank files
5. **End of Session**: Update progress and active context

### For Developers
1. **Onboarding**: Read projectbrief.md → productContext.md → systemPatterns.md → techContext.md
2. **Feature Work**: Review activeContext.md and relevant task files
3. **Code Review**: Reference established patterns in systemPatterns.md
4. **Documentation**: Update memory bank files when making significant changes

### Memory Bank File Templates

#### projectbrief.md Template
```markdown
# [PROJECT_NAME] - Project Brief

## Vision
[High-level project vision and goals]

## Target Users
[Primary user groups and their needs]

## Core Domain
[Domain terminology and key concepts]

## Architecture Overview
[High-level technical architecture decisions]

## Success Metrics
[How success is measured]
```

#### productContext.md Template
```markdown
# [PROJECT_NAME] - Product Context

## Domain Knowledge
[Business domain expertise and terminology]

## Business Rules
[Key business rules that affect implementation]

## User Workflows
[Primary user journeys and use cases]

## Feature Priorities
[Current feature priorities and roadmap]
```

#### systemPatterns.md Template
```markdown
# [PROJECT_NAME] - System Patterns

## Architecture Patterns
[Established architectural patterns and rationale]

## Code Patterns
[Common code patterns and examples]

## Data Patterns
[Data modeling and access patterns]

## Testing Patterns
[Testing strategies and practices]
```

#### techContext.md Template
```markdown
# [PROJECT_NAME] - Technical Context

## Technology Stack
[Detailed technology choices and versions]

## Development Environment
[Setup and configuration details]

## Build & Deployment
[Build processes and deployment procedures]

## Integrations
[Third-party services and APIs]
```

## Best Practices

### Writing Effective Memory Bank Files
1. **Be Specific**: Include concrete examples and code snippets
2. **Be Current**: Keep information up-to-date with actual implementation
3. **Be Complete**: Cover all aspects that affect development decisions
4. **Be Accessible**: Write for both AI and human readers

### Maintaining Consistency
1. **Cross-Reference**: Link related concepts across files
2. **Validate**: Ensure information matches actual codebase
3. **Review**: Regularly review and update for accuracy
4. **Version**: Track changes to understand evolution

### Common Pitfalls to Avoid
1. **Outdated Information**: Keep files current with codebase changes
2. **Too Abstract**: Include concrete, actionable details
3. **Missing Context**: Explain the "why" behind decisions
4. **Inconsistent Terminology**: Use consistent language across files

Remember: The Memory Bank is the project's single source of truth for context, patterns, and decisions. It should be comprehensive enough that anyone (including AI) can understand and contribute to the project effectively after reading it.