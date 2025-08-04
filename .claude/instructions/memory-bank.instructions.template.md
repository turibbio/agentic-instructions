# Claude Code Memory Bank for [PROJECT_NAME]

This file provides context and persistent memory for Claude Code when working on the [PROJECT_NAME] project.

## Project Context

### Mission
[PROJECT_NAME] is [PROJECT_DESCRIPTION] - provide a comprehensive description of what your project does and who it serves.

### Problems Solved
- **[Problem 1]**: [Solution description]
- **[Problem 2]**: [Solution description]
- **[Problem 3]**: [Solution description]
- **[Problem 4]**: [Solution description]

### Technology Stack
- **Backend**: [BACKEND_TECH]
- **Frontend**: [FRONTEND_TECH]
- **Database**: [DATABASE_TECH]
- **Testing**: [TESTING_FRAMEWORK] for backend
- **Language**: [UI_LANGUAGE] for domain/UI, [TECH_LANGUAGE] for technical code

## Current Project Status

### Completed ✅
- Initial backend setup ([BACKEND_TECH])
- Initial frontend setup ([FRONTEND_TECH])
- Basic project structure
- [Other completed items]
- Migration to Claude Code with `.claude/instructions/` structure

### In Development 🚧
- Domain models ([CORE_ENTITIES])
- REST API endpoints
- Database schema [DATABASE_TECH]
- Base [FRONTEND_FRAMEWORK] components

### To Implement 📋
- Authentication system
- [Feature 1]
- [Feature 2]
- [Feature 3]
- Data export ([EXPORT_FORMATS])
- Deployment and containerization

## Architectural Decisions

### Backend
- **[API_STYLE]** preferred for simple endpoints
- **[ALTERNATIVE_API_STYLE]** for complex logic
- **[PATTERN_NAME] pattern** with [ORM_TECH]
- **Dependency Injection** native [BACKEND_FRAMEWORK]
- **API Response wrapper** standardized

### Frontend
- **Functional components** with hooks
- **TypeScript strict mode** enabled
- **[STYLING_APPROACH]** for isolated styling
- **[STATE_MANAGEMENT]** for global state
- **Custom hooks** for reusable logic

### Database
- **[DATABASE_TECH]** as main database
- **[ORM_TECH]** for ORM
- **Code-First** approach for migrations
- **Naming convention** [NAMING_STYLE] for domain entities

## Domain Terminology

### Core Concepts (Always in [DOMAIN_LANGUAGE])
- **[CORE_ENTITY_1]**: [Description]
- **[CORE_ENTITY_2]**: [Description]
- **[CORE_ENTITY_3]**: [Description]
- **[CORE_ENTITY_4]**: [Description]
- **[CORE_ENTITY_5]**: [Description]
- **[CORE_ENTITY_6]**: [Description]
- **[CORE_ENTITY_7]**: [Description]

### [Entity Types/Categories]
- **[TYPE_1]**: [Description]
- **[TYPE_2]**: [Description]
- **[TYPE_3]**: [Description]
- **[TYPE_4]**: [Description]
- **[TYPE_5]**: [Description]

## Patterns and Conventions

### Naming Conventions
- **Backend ([BACKEND_LANGUAGE])**: [BACKEND_NAMING] for classes/methods, [BACKEND_VAR_NAMING] for variables
- **Frontend ([FRONTEND_FRAMEWORK])**: [FRONTEND_COMP_NAMING] for components, [FRONTEND_FUNC_NAMING] for functions
- **Database**: [DB_NAMING] for tables, [PROP_NAMING] for properties [BACKEND_LANGUAGE]
- **API Endpoints**: [URL_NAMING] in URLs, [DTO_NAMING] in DTOs

### Error Handling
- **API**: Standard HTTP codes + response wrapper
- **Frontend**: Error boundaries + user-friendly messages
- **Validation**: [VALIDATION_LIB] backend + form validation frontend
- **Logging**: Structured logging with [LOGGING_LIB]

### Testing Strategy
- **Backend**: Unit tests with [BACKEND_TEST_FRAMEWORK] + Integration tests
- **Frontend**: Component tests with [FRONTEND_TEST_FRAMEWORK] + [TESTING_LIB]
- **E2E**: [E2E_FRAMEWORK] for critical user journeys
- **API**: Automated tests with [API_DOC_TOOL]

## Established Technical Patterns

### API Response Pattern
```csharp
public record ApiResponse<T>
{
    public bool Success { get; init; }
    public T? Data { get; init; }
    public string? Message { get; init; }
    public IEnumerable<string>? Errors { get; init; }
    
    public static ApiResponse<T> Success(T data, string? message = null) =>
        new() { Success = true, Data = data, Message = message };
        
    public static ApiResponse<T> Error(string message, IEnumerable<string>? errors = null) =>
        new() { Success = false, Message = message, Errors = errors };
}
```

### Entity Base Pattern
```csharp
public abstract class BaseEntity
{
    public int Id { get; set; }
    public DateTime [CreatedProperty] { get; set; } = DateTime.UtcNow;
    public DateTime? [UpdatedProperty] { get; set; }
    public bool [DeletedProperty] { get; set; } = false;
    
    public void MarkAsUpdated() => [UpdatedProperty] = DateTime.UtcNow;
    public void SoftDelete() => [DeletedProperty] = true;
}
```

### React Hook Pattern
```typescript
export const use[Entity]Data = ([entity]Id: number) => {
    const [data, setData] = useState<[Entity] | null>(null);
    const [isLoading, setIsLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);
    
    useEffect(() => {
        const loadData = async () => {
            try {
                setIsLoading(true);
                const [entity] = await [entity]Service.get[Entity]([entity]Id);
                setData([entity]);
            } catch (err) {
                setError(err instanceof Error ? err.message : 'Unknown error');
            } finally {
                setIsLoading(false);
            }
        };
        
        loadData();
    }, [[entity]Id]);
    
    return { data, isLoading, error };
};
```

### Repository Pattern
```csharp
public interface IRepository<T> where T : BaseEntity
{
    Task<T?> GetByIdAsync(int id, CancellationToken cancellationToken = default);
    Task<IEnumerable<T>> GetAllAsync(CancellationToken cancellationToken = default);
    Task<T> CreateAsync(T entity, CancellationToken cancellationToken = default);
    Task<T?> UpdateAsync(T entity, CancellationToken cancellationToken = default);
    Task<bool> DeleteAsync(int id, CancellationToken cancellationToken = default);
}

public class Repository<T> : IRepository<T> where T : BaseEntity
{
    private readonly [DbContext] _context;
    private readonly DbSet<T> _dbSet;
    
    public Repository([DbContext] context)
    {
        _context = context;
        _dbSet = context.Set<T>();
    }
    
    public async Task<T?> GetByIdAsync(int id, CancellationToken cancellationToken = default)
    {
        return await _dbSet
            .Where(e => !e.[DeletedProperty])
            .FirstOrDefaultAsync(e => e.Id == id, cancellationToken);
    }
    
    // Other method implementations...
}
```

## Performance Guidelines

### Database Optimization
- **Eager Loading**: Use `Include()` only for necessary data
- **Pagination**: Always implement for lists > [PAGE_SIZE] elements
- **Indexes**: Add indexes on foreign keys and frequently filtered fields
- **Query Splitting**: For complex Includes use `AsSplitQuery()`

### Frontend Optimization
- **Code Splitting**: Use lazy loading for non-critical routes
- **Memoization**: `React.memo` for expensive components
- **Virtualization**: For lists > [VIRTUALIZATION_THRESHOLD] elements
- **Bundle Analysis**: Monitor bundle sizes regularly

### API Performance
- **Async/Await**: Always for I/O operations
- **Cancellation Tokens**: Support request cancellation
- **Response Compression**: Enable Gzip/Brotli
- **Rate Limiting**: Protect from abuse

## Security Best Practices

### Authentication & Authorization
- **JWT Tokens**: Short-lived access + refresh token pattern
- **Role-Based**: [ROLE_1], [ROLE_2], [ROLE_3]
- **Permission-Based**: Granular for specific operations
- **Secure Storage**: HttpOnly cookies for refresh tokens

### Input Validation
- **Server-Side**: Always validate server-side
- **Sanitization**: Escape HTML user input
- **SQL Injection**: Use parameterized queries ([ORM_TECH] handles this)
- **CSRF Protection**: CSRF tokens for state-changing operations

### Data Protection
- **[PRIVACY_REGULATION] Compliance**: [Compliance requirements]
- **PII Encryption**: Sensitive data encrypted at rest
- **Audit Logging**: Log sensitive operations
- **Backup Encryption**: Database backups encrypted

## Known Challenges

### Domain Specific
- **[Domain Challenge 1]**: [Description and approach]
- **[Domain Challenge 2]**: [Description and approach]
- **[Domain Challenge 3]**: [Description and approach]
- **[Domain Challenge 4]**: [Description and approach]

### Technical
- **Performance**: [Performance challenges and solutions]
- **Scalability**: [Scalability challenges and solutions]
- **[Regulation]**: [Regulatory compliance challenges]
- **Mobile**: [Mobile-specific considerations]

## Established Best Practices

### Development Workflow
1. **Feature branches** for each development
2. **Conventional commits** for automatic changelog
3. **Code review** mandatory before merge
4. **Automated testing** on every push
5. **Documentation** always updated

### Code Quality
- **Self-documenting code** preferred over comments
- **SOLID principles** applied consistently
- **DRY principle** but not at readability expense
- **TypeScript strict** without using `any`
- **Null safety** with nullable reference types [BACKEND_FRAMEWORK]

## Notes for Claude Code

### Reading Priority
1. **[PROJECT_README]** - Setup and basic commands
2. **This file** - Context and decisions
3. **`.claude/instructions/`** - Specific guidelines
4. **Project README** - User documentation

### Recommended Approach
- **Domain first**: Always start with business logic
- **Iterative**: Implement MVP before optimizations
- **User-centric**: Always think about the end user
- **[Domain Language]-friendly**: Prefer [DOMAIN_LANGUAGE] terminology in domain

### Essential Commands
```bash
# Backend
cd backend && [TEST_COMMAND] && [RUN_COMMAND]

# Frontend  
cd frontend && [LINT_COMMAND] && [DEV_COMMAND]

# Full stack
# Backend on :[BACKEND_PORT], Frontend on :[FRONTEND_PORT]
```

---
**Last updated**: [CURRENT_DATE]  
**Claude version**: [CLAUDE_VERSION]