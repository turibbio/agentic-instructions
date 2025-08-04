# GitHub Copilot Instructions for [PROJECT_NAME]

This directory contains custom GitHub Copilot instructions to enhance the development experience in the [PROJECT_NAME] project.

## Instruction Files Overview

### **Core Development Instructions**
| File | Description | ApplyTo |
|------|-------------|---------|
| `general.instructions.md` | General instructions, conventions and project overview | All project files |
| `backend.instructions.md` | Core [BACKEND_TECH] architecture and patterns | [BACKEND_FILES] |
| `frontend.instructions.md` | [FRONTEND_TECH] specifics | [FRONTEND_FILES] |
| `domain.instructions.md` | [DOMAIN_CONTEXT] terminology and business logic | Models, Types, Services |
| `[LANGUAGE].instructions.md` | Modern [LANGUAGE] features and language patterns | [LANGUAGE_FILES] |

### **Specialized Backend Instructions**
| File | Description | ApplyTo |
|------|-------------|---------|
| `validation-error-handling.instructions.md` | Advanced validation, Problem Details, error handling | Backend files |
| `caching-performance.instructions.md` | Caching strategies, performance optimization, rate limiting | Backend files, Program.cs |
| `monitoring-observability.instructions.md` | Structured logging, health checks, metrics, correlation ID | Backend files, config |
| `[BACKEND_API].instructions.md` | RESTful API design principles and implementation | API files, JSON config |
| `[BACKEND_ARCH].instructions.md` | DDD, SOLID principles, architecture patterns | Architecture files |

### **Testing & Quality**
| File | Description | ApplyTo |
|------|-------------|---------|
| `testing.instructions.md` | Testing strategies and patterns ([TEST_FRAMEWORKS]) | Test files and specs |
| `security-and-owasp.instructions.md` | Security best practices and OWASP guidelines | All applicable files |
| `object-calisthenics.instructions.md` | Code quality and object-oriented design principles | Programming files |
| `self-explanatory-code-commenting.instructions.md` | Code commenting and documentation standards | All code files |

### **DevOps & Infrastructure**
| File | Description | ApplyTo |
|------|-------------|---------|
| `devops-core-principles.instructions.md` | DevOps culture, CALMS framework, DORA metrics | All files |
| `containerization-docker-best-practices.instructions.md` | Docker optimization, security, and best practices | All files |
| `github-actions-ci-cd-best-practices.instructions.md` | CI/CD pipeline patterns and GitHub Actions | All files |
| `performance-optimization.instructions.md` | Performance optimization strategies | All applicable files |

### **Frontend Specialized**
| File | Description | ApplyTo |
|------|-------------|---------|
| `[FRONTEND_FRAMEWORK].instructions.md` | [FRONTEND_FRAMEWORK] development standards and best practices | [FRONTEND_EXTENSIONS] |
| `[E2E_FRAMEWORK].instructions.md` | E2E testing with [E2E_FRAMEWORK] | Test files |

### **Documentation & Workflow**
| File | Description | ApplyTo |
|------|-------------|---------|
| `markdown.instructions.md` | Documentation and Markdown formatting standards | Markdown files |
| `conventional-commit.prompt.md` | Conventional commit message guidelines | Git workflow |
| `spec-driven-workflow-v1.instructions.md` | Specification-driven development workflow | All applicable files |
| `localization.instructions.md` | Internationalization and localization patterns | All applicable files |

### **Meta & Configuration**
| File | Description | ApplyTo |
|------|-------------|---------|
| `copilot-thought-logging.instructions.md` | Copilot process tracking and interaction guidelines | All files |
| `taming-copilot.instructions.md` | Advanced Copilot usage and control techniques | All files |
| `memory-bank.instructions.md` | AI memory management and session continuity patterns | All files |
| `usage-guide.md` | Complete guide on how to use these instructions | - |

## Key Features After Refactoring

### ✅ **Specialized Architecture**
- **Eliminated Duplications**: Removed ~60% of redundant content
- **Focused Instructions**: Each file has a specific, well-defined purpose
- **Cross-References**: Clear links between related instruction files
- **Centralized Expertise**: Advanced topics consolidated in specialized files

### ✅ **New Specialized Files**
- **`validation-error-handling.instructions.md`**: Advanced validation patterns, Problem Details (RFC 7807)
- **`caching-performance.instructions.md`**: Comprehensive caching strategies and performance optimization
- **`monitoring-observability.instructions.md`**: Structured logging, health checks, correlation IDs, metrics

### ✅ **Enhanced Backend Instructions**
- **Core Backend**: Focused on fundamental architecture and patterns
- **Validation & Error Handling**: Advanced validation, custom exceptions, Result patterns
- **Performance & Caching**: Caching strategies, pagination, rate limiting
- **Monitoring**: Structured logging, health checks, performance tracking

### ✅ **Improved Organization**
- **Domain-Driven**: Business logic and domain terminology preserved
- **Technology-Specific**: Clear separation between backend, frontend, and cross-cutting concerns
- **ApplyTo Configurations**: Precise file targeting for relevant suggestions
- **Modern Patterns**: Latest technology features and best practices

## Project Objectives

- ✅ **Code Consistency**: Unified naming and patterns
- ✅ **Facilitated Onboarding**: More productive new developers
- ✅ **Improved Quality**: Contextual and appropriate suggestions
- ✅ **Domain Terminology**: Consistency with application domain
- ✅ **Best Practices**: Architectural patterns and security
- ✅ **Guided Testing**: Domain-specific test patterns
- ✅ **Specialized Configuration**: Targeted instructions for different file types
- ✅ **Performance Focus**: Optimization patterns and monitoring
- ✅ **Advanced Error Handling**: Comprehensive validation and error management

## Getting Started

1. **Ensure GitHub Copilot is active** in your editor
2. **Read the usage guide**: `usage-guide.md`
3. **Explore examples** in specific instructions
4. **Start coding** - Copilot will automatically use the instructions

## Contributing

To improve the instructions:

1. Identify new patterns or scenarios
2. Add specific examples to relevant specialized files
3. Propose changes via PR
4. Maintain consistency with existing terminology
5. Consider cross-references between related instruction files

## Advanced Features

### ApplyTo Configurations
Each instruction file includes `applyTo` configurations that specify:
- File patterns to apply the instructions to
- Specific targeting for different technologies
- Reduced "noise" for non-relevant suggestions

### Specialized Instruction Files
The new architecture provides:
- **Backend Specialization**: Core, validation, caching, monitoring
- **Frontend Focus**: Modern framework patterns
- **Cross-Cutting Concerns**: Security, testing, DevOps
- **Domain Integration**: Business rules and domain terminology

### Domain-Specific Code Examples
All files now include practical examples for:
- Business rule validators
- Domain-specific services
- Entity management patterns
- Context-appropriate formatting

## Architecture Overview

**Total Files: 25+ instruction files** organized by concern and complexity:

```
Core Instructions (5 files)
├── backend.instructions.md              # Fundamental backend patterns
├── frontend.instructions.md             # Frontend framework essentials
├── domain.instructions.md               # Business domain terminology
├── general.instructions.md              # Project-wide conventions
└── [language].instructions.md           # Modern language features

Specialized Backend (5 files)
├── validation-error-handling.instructions.md    # Advanced validation
├── caching-performance.instructions.md          # Performance optimization
├── monitoring-observability.instructions.md     # Logging & monitoring
├── [backend-api].instructions.md               # REST API patterns
└── [backend-arch].instructions.md              # Architecture principles

Quality & Testing (4 files)
├── testing.instructions.md              # Test patterns
├── security-and-owasp.instructions.md   # Security best practices
├── object-calisthenics.instructions.md  # Code quality
└── self-explanatory-code-commenting.instructions.md  # Documentation

DevOps & Infrastructure (4 files)
├── devops-core-principles.instructions.md       # DevOps culture
├── containerization-docker-best-practices.instructions.md
├── github-actions-ci-cd-best-practices.instructions.md
└── performance-optimization.instructions.md     # Performance strategies

Frontend Specialized (2 files)
├── [frontend-framework].instructions.md         # Framework best practices
└── [e2e-framework].instructions.md             # E2E testing

Documentation & Workflow (4 files)
├── markdown.instructions.md             # Documentation standards
├── conventional-commit.prompt.md        # Commit guidelines
├── spec-driven-workflow-v1.instructions.md      # Spec-driven development
└── localization.instructions.md         # i18n patterns

Meta & Configuration (4 files)
├── copilot-thought-logging.instructions.md      # Copilot process tracking
├── taming-copilot.instructions.md       # Advanced Copilot techniques
├── memory-bank.instructions.md          # AI memory management
└── usage-guide.md                       # Complete usage guide
```

## Support

For questions or issues:
- Consult the `usage-guide.md`
- Open an issue in the repository
- Propose improvements via PR
- Reference cross-linked specialized instruction files