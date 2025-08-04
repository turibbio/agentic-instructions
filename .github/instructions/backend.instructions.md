---
applyTo:
  - "backend/**/*.[BACKEND_EXT]"
  - "**/*.[PROJECT_EXT]"
  - "**/Program.[BACKEND_EXT]"
  - "**/appsettings*.[CONFIG_EXT]"
---

# Core Backend Instructions - [BACKEND_TECH]

## Fundamental Principles

### REST Architecture
- Follow REST principles for consistent API design
- Use object-oriented resources with meaningful URLs
- Correctly implement HTTP verbs (GET, POST, PUT, DELETE)
- Handle content negotiation and standardized response formats
- Design APIs thinking about external consumers and documentation

## Architecture

### Choosing Between Controllers and Minimal API

#### Minimal API (Recommended for [PROJECT_NAME])
Use [BACKEND_TECH] Minimal API for simple endpoints and better performance:

```[BACKEND_LANGUAGE]
// Grouping for better organization
var [entityGroup] = app.MapGroup("/api/[entities]")
    .WithTags("[Entities]")
    .WithOpenApi();

[entityGroup].MapGet("/", async (I[Entity]Service [entity]Service) =>
{
    var [entities] = await [entity]Service.Get[Entities]Async();
    return Results.Ok(new ApiResponse<IEnumerable<[Entity]>>
    {
        Success = true,
        Data = [entities]
    });
})
.WithName("Get[Entities]")
.WithSummary("Get all [entities]")
.Produces<ApiResponse<IEnumerable<[Entity]>>>();
```

#### Controller-Based API (For Complex Logic)
Use traditional controllers for complex logic and advanced validations:

```[BACKEND_LANGUAGE]
[ApiController]
[Route("api/[controller]")]
public class [Entities]Controller : ControllerBase
{
    private readonly I[Entity]Service _[entity]Service;

    public [Entities]Controller(I[Entity]Service [entity]Service)
    {
        _[entity]Service = [entity]Service;
    }

    [HttpGet("{id:int}")]
    [ProducesResponseType<ApiResponse<[Entity]>>(200)]
    [ProducesResponseType<ApiResponse<object>>(404)]
    public async Task<ActionResult<ApiResponse<[Entity]>>> Get[Entity](int id)
    {
        var [entity] = await _[entity]Service.Get[Entity]Async(id);
        if ([entity] == null)
        {
            return NotFound(new ApiResponse<object>
            {
                Success = false,
                Message = "[Entity] not found"
            });
        }

        return Ok(new ApiResponse<[Entity]>
        {
            Success = true,
            Data = [entity]
        });
    }
}
```

## Code Organization

### Domain Models
Define domain models with records for immutability:

```[BACKEND_LANGUAGE]
public record [Entity](
    int Id,
    string [Property1],
    [DateType] [DateProperty],
    string [Property2],
    int [NumericProperty]
);

public record [RelatedEntity](
    int Id,
    string [Property1],
    string [Property2],
    string [Property3],
    int? [OptionalProperty]
);
```

### Services and Dependency Injection
Use interfaces for services and dependency injection:

```[BACKEND_LANGUAGE]
public interface I[Entity]Service
{
    Task<IEnumerable<[Entity]>> Get[Entities]Async();
    Task<[Entity]?> Get[Entity]Async(int id);
    Task<[Entity]> Create[Entity]Async([Entity] [entity]);
}

public class [Entity]Service : I[Entity]Service
{
    private readonly I[Entity]Repository _repository;
    private readonly ILogger<[Entity]Service> _logger;
    
    public [Entity]Service(I[Entity]Repository repository, ILogger<[Entity]Service> logger)
    {
        _repository = repository;
        _logger = logger;
    }
    
    // Implementation
}
```

### Data Access Layer
Use Entity Framework Core with clean repository pattern:

```[BACKEND_LANGUAGE]
public class [PROJECT_NAME]DbContext : DbContext
{
    public DbSet<[Entity]> [Entities] { get; set; }
    public DbSet<[RelatedEntity]> [RelatedEntities] { get; set; }
    
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.ApplyConfigurationsFromAssembly(typeof([PROJECT_NAME]DbContext).Assembly);
    }
}

public interface I[Entity]Repository
{
    Task<IEnumerable<[Entity]>> Get[Entities]Async();
    Task<[Entity]?> GetByIdAsync(int id);
    Task<[Entity]> AddAsync([Entity] [entity]);
    Task UpdateAsync([Entity] [entity]);
    Task DeleteAsync(int id);
}
```

## Naming Conventions

### Project Structure
- **Namespace**: `[PROJECT_NAME].Api`, `[PROJECT_NAME].Core`, `[PROJECT_NAME].Data`
- **Controller**: `[Entities]Controller`, `[RelatedEntities]Controller`
- **Service**: `[Entity]Service`, `[RelatedEntity]Service`
- **Model**: `[Entity]`, `[RelatedEntity]`, `[AssociationEntity]`

### Coding Standards
- Use PascalCase for public members and classes
- Use camelCase for private fields and local variables
- Use meaningful names that reflect business domain
- Prefix interfaces with "I"

## API Design Standards

### REST Endpoint Conventions
Follow RESTful conventions for URLs:

```
GET    /api/[entities]              # List [entities]
GET    /api/[entities]/{id}         # Get [entity] details
POST   /api/[entities]              # Create [entity]
PUT    /api/[entities]/{id}         # Update [entity]
DELETE /api/[entities]/{id}         # Delete [entity]

POST   /api/[entities]/{id}/[related-action]    # [Related action]
GET    /api/[entities]/{id}/[related-entities]  # List [related entities]
```

### Response Format
Use consistent format for all responses:

```[BACKEND_LANGUAGE]
public record ApiResponse<T>
{
    public bool Success { get; init; }
    public T? Data { get; init; }
    public string? Message { get; init; }
    public IEnumerable<string>? Errors { get; init; }
}
```

### HTTP Status Codes
- `200 OK`: Successful operation
- `201 Created`: Resource created
- `400 Bad Request`: Validation error
- `404 Not Found`: Resource not found
- `409 Conflict`: Conflict (e.g., duplicate resource)
- `422 Unprocessable Entity`: Business rule violation
- `500 Internal Server Error`: Server error

## Configuration and Startup

### Strongly-Typed Configuration
```[BACKEND_LANGUAGE]
public class DatabaseOptions
{
    public const string SectionName = "Database";
    
    public string ConnectionString { get; set; } = string.Empty;
    public int CommandTimeout { get; set; } = 30;
}

// In Program.[BACKEND_EXT]
builder.Services.Configure<DatabaseOptions>(
    builder.Configuration.GetSection(DatabaseOptions.SectionName));
```

### Service Registration
```[BACKEND_LANGUAGE]
// Program.[BACKEND_EXT]
var builder = WebApplication.CreateBuilder(args);

// Add services
builder.Services.AddDbContext<[PROJECT_NAME]DbContext>(options =>
    options.[DATABASE_PROVIDER](builder.Configuration.GetConnectionString("DefaultConnection")));

builder.Services.AddScoped<I[Entity]Service, [Entity]Service>();
builder.Services.AddScoped<I[Entity]Repository, [Entity]Repository>();

// Add specialized configurations
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure pipeline
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseAuthentication();
app.UseAuthorization();

app.MapControllers();

app.Run();
```

## Security Configuration

### CORS Setup
```[BACKEND_LANGUAGE]
builder.Services.AddCors(options =>
{
    options.AddPolicy("Frontend", policy =>
    {
        policy.WithOrigins("[FRONTEND_URL]")
              .AllowAnyHeader()
              .AllowAnyMethod()
              .AllowCredentials();
    });
});

app.UseCors("Frontend");
```

### Basic Authentication Setup
```[BACKEND_LANGUAGE]
builder.Services.AddAuthentication([AUTH_SCHEME])
    .Add[AUTH_METHOD](options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = builder.Configuration["[AUTH_CONFIG]:Issuer"],
            ValidAudience = builder.Configuration["[AUTH_CONFIG]:Audience"],
            IssuerSigningKey = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(builder.Configuration["[AUTH_CONFIG]:Key"]!))
        };
    });
```

## Essential Anti-Patterns to Avoid

### Performance Anti-Patterns
- Don't use `Task.Wait()` or `Task.Result` in async context
- Avoid synchronous calls in async methods
- Don't forget to dispose resources (use `using` statements)

### Security Anti-Patterns
- Don't hardcode connection strings or secrets
- Avoid returning sensitive data in error messages
- Don't trust user input without validation

### Architecture Anti-Patterns
- Don't put business logic in controllers
- Avoid repository pattern if Entity Framework is sufficient
- Don't use generic repositories without clear benefits
- Avoid overly complex inheritance hierarchies

## References to Specialized Instructions

For advanced topics, refer to:
- **Validation and Error Handling**: `validation-error-handling.instructions.md`
- **Caching and Performance**: `caching-performance.instructions.md`
- **Monitoring and Observability**: `monitoring-observability.instructions.md`
- **Security and OWASP**: `security-and-owasp.instructions.md`
- **Testing Patterns**: `testing.instructions.md`