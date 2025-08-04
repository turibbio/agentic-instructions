# How to Use Copilot Instructions

This guide explains how to leverage the custom Copilot instructions for the [PROJECT_NAME] project effectively.

## Instruction Structure

The project includes specialized instruction files organized by concern:

### **Core Instructions**
- **`general.instructions.md`** - General instructions and project overview
- **`backend.instructions.md`** - Core [BACKEND_TECH] patterns
- **`frontend.instructions.md`** - [FRONTEND_TECH] specifics
- **`domain.instructions.md`** - [DOMAIN_CONTEXT] terminology and business logic
- **`[language].instructions.md`** - Modern [LANGUAGE] features

### **Specialized Backend Instructions**
- **`validation-error-handling.instructions.md`** - Advanced validation patterns
- **`caching-performance.instructions.md`** - Performance optimization and caching
- **`monitoring-observability.instructions.md`** - Logging, metrics, and health checks

### **Testing & Quality**
- **`testing.instructions.md`** - Testing patterns ([TEST_FRAMEWORKS])
- **`security-and-owasp.instructions.md`** - Security best practices

### **Meta & AI Tools**
- **`memory-bank.instructions.md`** - AI memory management and session continuity
- **`copilot-thought-logging.instructions.md`** - Copilot process tracking
- **`taming-copilot.instructions.md`** - Advanced Copilot usage techniques

## GitHub Copilot Configuration

### For Visual Studio Code

1. **Install GitHub Copilot Extension**
   ```bash
   code --install-extension GitHub.copilot
   ```

2. **Configure Custom Instructions**
   
   Open VS Code settings (`Ctrl+,` on Windows/Linux or `Cmd+,` on macOS) and add:
   ```json
   {
     "github.copilot.enable": {
       "*": true,
       "yaml": false,
       "plaintext": false,
       "markdown": false
     }
   }
   ```
   
   > **Note**: Custom instructions are automatically loaded from the `instructions/` folder when using GitHub Copilot in the project. Each file includes `applyTo` configurations for specific targeting.

3. **Reload VS Code** to apply the new instructions

### For JetBrains IDEs (Rider, IntelliJ)

1. **Install GitHub Copilot Plugin**
   - Go to `File > Settings > Plugins`
   - Search for "GitHub Copilot" and install it

2. **Instructions are automatically loaded** from instruction files

## How to Use the Instructions

### Contextual Code Suggestions

Instructions improve Copilot suggestions based on context:

#### Backend - Core Architecture
When working on backend files, Copilot will suggest:
```[BACKEND_LANGUAGE]
// Writing "create [ENTITY] service" will get you:
public interface I[Entity]Service
{
    Task<IEnumerable<[Entity]>> Get[Entities]Async();
    Task<[Entity]?> Get[Entity]Async(int id);
    Task<[Entity]> Create[Entity]Async(Create[Entity]Request request);
}
```

#### Backend - Advanced Validation
For validation scenarios:
```[BACKEND_LANGUAGE]
// Writing "validate [entity] request" will get you:
public class Create[Entity]RequestValidator : AbstractValidator<Create[Entity]Request>
{
    public Create[Entity]RequestValidator()
    {
        RuleFor(x => x.[Property])
            .NotEmpty().WithMessage("[Property] is required")
            .Length(3, 100).WithMessage("[Property] must be between 3 and 100 characters");

        RuleFor(x => x.[DateProperty])
            .GreaterThan(DateOnly.FromDateTime(DateTime.Now.AddDays(7)))
            .WithMessage("[Entity] must be scheduled at least 7 days in the future");
    }
}
```

#### Backend - Caching and Performance
For performance optimization:
```[BACKEND_LANGUAGE]
// Writing "cached [entity] service" will get you:
public class Cached[Entity]Service : I[Entity]Service
{
    private readonly I[Entity]Service _baseService;
    private readonly IDistributedCache _cache;
    
    public async Task<IEnumerable<[Entity]>> Get[Entities]Async()
    {
        const string cacheKey = "[entities]_list";
        
        var cached[Entities] = await _cache.GetStringAsync(cacheKey);
        if (cached[Entities] != null)
        {
            return JsonSerializer.Deserialize<IEnumerable<[Entity]>>(cached[Entities])!;
        }

        var [entities] = await _baseService.Get[Entities]Async();
        
        await _cache.SetStringAsync(cacheKey, JsonSerializer.Serialize([entities]), 
            new DistributedCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5)
            });

        return [entities];
    }
}
```

#### Backend - Monitoring and Logging
For observability patterns:
```[BACKEND_LANGUAGE]
// Writing "structured logging service" will get you:
public class [Entity]Service : I[Entity]Service
{
    private readonly ILogger<[Entity]Service> _logger;
    
    public async Task<[Entity]> Create[Entity]Async(Create[Entity]Request request)
    {
        _logger.LogInformation("Creating [entity] {[Entity]Name} for {MaxItems} items", 
            request.[Property], request.Max[Items]);
            
        try
        {
            var [entity] = await _repository.AddAsync(request);
            _logger.LogInformation("[Entity] created successfully with ID {[Entity]Id}", [entity].Id);
            return [entity];
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to create [entity] {[Entity]Name}", request.[Property]);
            throw;
        }
    }
}
```

#### Testing
When working on test files, Copilot will suggest:
```[BACKEND_LANGUAGE]
// For backend tests:
[Test]
public async Task Create[Entity]_ValidInput_Returns[Entity]()
{
    // Arrange
    var request = new Create[Entity]Request { [Property] = "Test [Entity]" };
    
    // Act
    var result = await _[entity]Service.Create[Entity]Async(request);
    
    // Assert
    Assert.That(result.[Property], Is.EqualTo("Test [Entity]"));
}
```

#### Frontend
When working on frontend files, Copilot will suggest:
```[FRONTEND_LANGUAGE]
// Writing "[entity] card component" will get you:
interface [Entity]CardProps {
  [entity]: [Entity];
  on[Action]: ([entity]Id: number) => void;
}

export const [Entity]Card: React.FC<[Entity]CardProps> = ({ [entity], on[Action] }) => {
  return (
    <div className="[entity]-card">
      <h3>{[entity].[property]}</h3>
      <p>Date: {[entity].[dateProperty].toLocaleDateString('[LOCALE]')}</p>
      <button onClick={() => on[Action]([entity].id)}>
        [Action_Label]
      </button>
    </div>
  );
};
```

### Effective Prompts

Use these prompts to get the best results:

#### For Backend Core
```
// Create a service to manage [domain entities]
// Implement validation for [domain entity] [action]
// Create an endpoint to get [domain entity] [data]
```

#### For Backend Validation
```
// Advanced validation for [entity] creation
// Custom validation rules for [entity property]
// Problem Details error response for business rule violations
```

#### For Backend Performance
```
// Cached service with [CACHE_TECHNOLOGY] for [entity] data
// Pagination helper for large [entity] lists
// Rate limiting for [entity] creation endpoints
```

#### For Backend Monitoring
```
// Structured logging with correlation ID
// Health check for [entity] service
// Performance metrics for API endpoints
```

#### For Tests
```
// Unit test for [entity] validation business rules
// Mock repository for [entities] with [TEST_FRAMEWORK]
// Integration test for [entity] API endpoints
```

#### For Frontend
```
// Create [action] form for [domain entities]
// Implement [entity] list with search functionality
// Create component to display [domain data]
```

#### For APIs
```
// REST endpoint to manage [domain entities]
// Validation for [entity] [action] data
// Response format for [domain results]
```

## Practical Examples

### Scenario 1: Advanced Validation Component

**Prompt**: `Create comprehensive validation for [entity] [action] with business rules`

**Expected Result**:
```[BACKEND_LANGUAGE]
public class [Entity]BusinessRules
{
    public static ValidationResult Validate[Action]([Entity] [entity], Create[Entity]Request request)
    {
        var errors = new List<string>();
        
        // Check business rule 1
        if ([condition1])
        {
            errors.Add("[Business rule violation message]");
        }
        
        // Check business rule 2
        if ([condition2])
        {
            errors.Add("[Business rule violation message]");
        }
        
        // Check business rule 3
        var [calculatedValue] = Calculate[Value](request.[Property], [entity].[Property]);
        if ([calculatedValue] < [minimumValue])
        {
            errors.Add("[Minimum value validation message]");
        }
        
        return new ValidationResult
        {
            IsValid = errors.Count == 0,
            Errors = errors
        };
    }
}
```

### Scenario 2: Performance-Optimized Service

**Prompt**: `High-performance [entity] service with caching and monitoring`

**Expected Result**:
```[BACKEND_LANGUAGE]
public class Performant[Entity]Service : I[Entity]Service
{
    private readonly I[Entity]Repository _repository;
    private readonly IDistributedCache _cache;
    private readonly ILogger<Performant[Entity]Service> _logger;
    private readonly PerformanceMetrics _metrics;

    public async Task<PagedResult<[Entity]>> Get[Entities]PagedAsync(int page, int pageSize)
    {
        using var activity = ActivitySource.StartActivity("Get[Entities]Paged");
        activity?.SetTag("page", page);
        activity?.SetTag("pageSize", pageSize);
        
        var stopwatch = Stopwatch.StartNew();
        
        try
        {
            var cacheKey = $"[entities]_page_{page}_{pageSize}";
            var cached = await _cache.GetStringAsync(cacheKey);
            
            if (cached != null)
            {
                _metrics.RecordCacheHit();
                _logger.LogDebug("[Entities] page retrieved from cache");
                return JsonSerializer.Deserialize<PagedResult<[Entity]>>(cached)!;
            }
            
            _metrics.RecordCacheMiss();
            var result = await _repository.Get[Entities]PagedAsync(page, pageSize);
            
            await _cache.SetStringAsync(cacheKey, JsonSerializer.Serialize(result),
                new DistributedCacheEntryOptions
                {
                    AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5)
                });
            
            return result;
        }
        finally
        {
            stopwatch.Stop();
            _metrics.RecordRequest(stopwatch.Elapsed.TotalSeconds);
        }
    }
}
```

### Scenario 3: Advanced Error Handling

**Prompt**: `Comprehensive error handling with Problem Details`

**Expected Result**:
```[BACKEND_LANGUAGE]
public class GlobalExceptionHandlingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<GlobalExceptionHandlingMiddleware> _logger;

    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "An unhandled exception occurred");
            await HandleExceptionAsync(context, ex);
        }
    }

    private static async Task HandleExceptionAsync(HttpContext context, Exception exception)
    {
        context.Response.ContentType = "application/problem+json";

        var problemDetails = exception switch
        {
            ValidationException validationEx => new ValidationProblemDetails
            {
                Title = "Validation failed",
                Status = 400,
                Detail = "One or more validation errors occurred",
                Instance = context.Request.Path
            },
            
            [DomainException] [domainEx] => new ProblemDetails
            {
                Title = "[Domain specific error]",
                Status = 422,
                Detail = [domainEx].Message,
                Instance = context.Request.Path
            },
            
            _ => new ProblemDetails
            {
                Title = "An error occurred",
                Status = 500,
                Detail = "An unexpected error occurred",
                Instance = context.Request.Path
            }
        };

        context.Response.StatusCode = problemDetails.Status ?? 500;
        
        var json = JsonSerializer.Serialize(problemDetails, new JsonSerializerOptions
        {
            PropertyNamingPolicy = JsonNamingPolicy.CamelCase
        });
        
        await context.Response.WriteAsync(json);
    }
}
```

### Scenario 4: Frontend Custom Hook

**Prompt**: `Custom hook for managing [entity] data with caching`

**Expected Result**:
```[FRONTEND_LANGUAGE]
export const use[Entity]Data = ([entity]Id: number) => {
  const [[entity], set[Entity]] = useState<[Entity] | null>(null);
  const [[relatedEntities], set[RelatedEntities]] = useState<[RelatedEntity][]>([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const load[Entity]Data = useCallback(async () => {
    try {
      setIsLoading(true);
      setError(null);
      
      const [[entity]Data, [relatedEntities]Data] = await Promise.all([
        [entity]Service.get[Entity]([entity]Id),
        [entity]Service.get[RelatedEntities]([entity]Id)
      ]);
      
      set[Entity]([entity]Data);
      set[RelatedEntities]([relatedEntities]Data);
    } catch (err) {
      setError('Error loading [entity] data');
      console.error('Failed to load [entity] data:', err);
    } finally {
      setIsLoading(false);
    }
  }, [[entity]Id]);

  useEffect(() => {
    load[Entity]Data();
  }, [load[Entity]Data]);

  const [actionMethod] = async (data[Entity]: Create[Entity]Request) => {
    try {
      const new[Entity] = await [entity]Service.[actionMethod]([entity]Id, data[Entity]);
      set[RelatedEntities](prev => [...prev, new[Entity].[relatedEntity]]);
      return new[Entity];
    } catch (err) {
      throw new Error('Failed to [action] [entity]');
    }
  };

  return { 
    [entity], 
    [relatedEntities], 
    isLoading, 
    error, 
    [actionMethod], 
    reload: load[Entity]Data 
  };
};
```

## Tips for Maximum Effectiveness

### 1. Be Specific with Domain Context
```
❌ "Create a [generic] form"
✅ "Create [specific] form for [domain entities] with [context] validation"
```

### 2. Use Project Terminology
```
❌ "[generic entity] list component"
✅ "[specific entity] list component with [domain properties] and [domain actions]"
```

### 3. Specify Technical Context
```
❌ "create api endpoint"
✅ "[API_TYPE] endpoint for managing [domain entities] with caching"
```

### 4. Include Business Requirements
```
❌ "validate form"
✅ "validate [entity] form with [domain] rules and [validation framework]"
```

### 5. Specify Test Type and Framework
```
❌ "test this function"
✅ "[TEST_FRAMEWORK] unit test for [entity] [business rule] validation with mocks"
```

### 6. Include Performance Considerations
```
❌ "get [entities] list"
✅ "get [entities] list with [cache technology] caching and pagination"
```

### 7. Specify Error Handling Approach
```
❌ "handle errors"
✅ "handle errors with Problem Details RFC 7807 and structured logging"
```

## Troubleshooting

### Copilot Not Following Instructions

1. **Check file location**: Files must be in `.github/instructions/` folder
2. **Verify applyTo patterns**: Ensure the file you're working on matches the `applyTo` patterns
3. **Reload editor**: Close and reopen VS Code/IDE
4. **Check permissions**: Ensure Copilot has repository access
5. **Be more specific**: Use terms present in the relevant instruction files

### Non-Contextual Suggestions

1. **Use descriptive comments** before code
2. **Name files clearly** (`[Entity]Service.[ext]`, `[Entity]Card.[ext]`)
3. **Maintain consistency** with existing patterns
4. **Reference specific instruction files** for advanced scenarios

### Low Quality Suggestions

1. **Update instructions** if specific patterns are missing
2. **Provide more context** in prompts
3. **Use concrete examples** in instruction files
4. **Cross-reference specialized files** for advanced topics

## Specialized Instruction Usage

### For Advanced Backend Scenarios
- **Validation**: Reference `validation-error-handling.instructions.md`
- **Performance**: Reference `caching-performance.instructions.md`
- **Monitoring**: Reference `monitoring-observability.instructions.md`

### For Security Scenarios
- Reference `security-and-owasp.instructions.md`
- Include security context in prompts

### For Testing Scenarios
- Reference `testing.instructions.md`
- Specify test framework and test type

### For AI Memory Management
- Reference `memory-bank.instructions.md`
- Use for session continuity and project context management
- Supports task tracking and progress documentation

## Contributing to Instructions

To improve the instructions:

1. **Identify recurring patterns** in your code
2. **Add specific examples** to relevant specialized files
3. **Document best practices** discovered during development
4. **Maintain consistency** with existing terminology
5. **Create cross-references** between related instruction files

## Success Metrics

Monitor instruction effectiveness:

- **Reduced time** for writing boilerplate code
- **Greater consistency** in naming conventions
- **Fewer errors** in following conventions
- **Facilitated onboarding** for new developers
- **Improved test coverage** with guided patterns
- **Correct domain terminology** usage
- **Proper use of advanced patterns** (caching, validation, monitoring)

## New Features Introduced

### Specialized Architecture
Each instruction file now has a specific focus:
- `backend.instructions.md`: Core backend patterns only
- `validation-error-handling.instructions.md`: Advanced validation and error handling
- `caching-performance.instructions.md`: Performance optimization and caching
- `monitoring-observability.instructions.md`: Logging, metrics, and health checks

### Cross-Referenced Instructions
Files now reference each other for advanced topics:
- Core files point to specialized files for advanced scenarios
- Specialized files provide deep implementation details
- Clear separation of concerns

### Enhanced Examples
Each specialized file includes:
- Real-world implementation patterns
- Domain-specific business logic
- Performance optimization techniques
- Comprehensive error handling strategies