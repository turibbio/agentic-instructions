---
applyTo:
  - "backend/**/*.cs"
  - "**/*.csproj"
  - "**/Program.cs"
  - "**/appsettings*.json"
---

# Core Backend Instructions - [BACKEND_FRAMEWORK]

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
Use [BACKEND_FRAMEWORK] Minimal API for simple endpoints and better performance:

```csharp
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

```csharp
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

```csharp
public record [Entity](
    int Id,
    string [Property1],
    DateOnly [Property2],
    string [Property3],
    int [Property4]
);

public record [RelatedEntity](
    int Id,
    string [Property1],
    string [Property2],
    string [Property3],
    int? [Property4]
);
```

### Services and Dependency Injection
Use interfaces for services and dependency injection:

```csharp
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

```csharp
public class [ProjectName]DbContext : DbContext
{
    public DbSet<[Entity]> [Entities] { get; set; }
    public DbSet<[RelatedEntity]> [RelatedEntities] { get; set; }
    
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.ApplyConfigurationsFromAssembly(typeof([ProjectName]DbContext).Assembly);
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
- **Namespace**: `[ProjectName].Api`, `[ProjectName].Core`, `[ProjectName].Data`
- **Controller**: `[Entities]Controller`, `[RelatedEntities]Controller`
- **Service**: `[Entity]Service`, `[RelatedEntity]Service`
- **Model**: `[Entity]`, `[RelatedEntity]`, `[Action]`

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

POST   /api/[entities]/{id}/[actions]    # Perform [action]
GET    /api/[entities]/{id}/[related]    # List [related] items
```

### Response Format
Use consistent format for all responses:

```csharp
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
```csharp
public class DatabaseOptions
{
    public const string SectionName = "Database";
    
    public string ConnectionString { get; set; } = string.Empty;
    public int CommandTimeout { get; set; } = 30;
}

// In Program.cs
builder.Services.Configure<DatabaseOptions>(
    builder.Configuration.GetSection(DatabaseOptions.SectionName));
```

### Service Registration
```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);

// Add services
builder.Services.AddDbContext<[ProjectName]DbContext>(options =>
    options.Use[DatabaseProvider](builder.Configuration.GetConnectionString("DefaultConnection")));

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
```csharp
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
```csharp
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = builder.Configuration["Jwt:Issuer"],
            ValidAudience = builder.Configuration["Jwt:Audience"],
            IssuerSigningKey = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Key"]!))
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