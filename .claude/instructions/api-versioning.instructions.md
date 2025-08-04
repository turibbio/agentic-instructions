# API Versioning Instructions

## Versioning Strategy

### Semantic Versi        // From Accept header: Accept: application/vnd.api.v1.2+json
        var acceptHeader = Request.Headers.Accept.FirstOrDef        T        Title = "Project API V2", 
        Version = "v2", 
        Description = "Project API Version 2 - Enhanced features"e = "Project API V1", 
        Version = "v1", 
        Description = "Project API Version 1 - Stable release"t()?.ToString();
        if (acceptHeader?.Contains("vnd.api") == true)
        {
            var match = Regex.Match(acceptHeader, @"vnd\.api\.v(\d+)\.(\d+)");
- **Major (v1, v2, v3)**: Breaking changes requiring client updates
- **Minor (v1.1, v1.2)**: New features, backward compatible
- **Patch (v1.1.1, v1.1.2)**: Bug fixes, fully backward compatible

### Versioning Approach
Use **URL Path Versioning** for major versions, **Header Versioning** for minor versions:
- Major: `/api/v1/eventi`, `/api/v2/eventi`
- Minor: `Accept: application/vnd.api.v1.1+json`

## Implementation Patterns

### URL Path Versioning (Recommended)
```csharp
// Program.cs - Version routing setup
app.MapGroup("/api/v1")
   .WithTags("V1")
   .MapV1Endpoints();

app.MapGroup("/api/v2")
   .WithTags("V2")
   .MapV2Endpoints();

// Endpoints organization
public static class V1Endpoints
{
    public static void MapV1Endpoints(this IEndpointRouteBuilder app)
    {
        // V1 specific endpoints
        app.MapGet("/eventi", GetEventiV1)
           .WithName("GetEventi_V1")
           .WithSummary("Get events - Version 1");
    }
}

public static class V2Endpoints
{
    public static void MapV2Endpoints(this IEndpointRouteBuilder app)
    {
        // V2 enhanced endpoints
        app.MapGet("/eventi", GetEventiV2)
           .WithName("GetEventi_V2")
           .WithSummary("Get events - Version 2 with enhanced filtering");
    }
}
```

### Header-Based Versioning (Alternative)
```csharp
// Custom middleware for version detection
public class ApiVersionMiddleware
{
    private readonly RequestDelegate _next;

    public ApiVersionMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var apiVersion = ExtractApiVersion(context.Request);
        context.Items["ApiVersion"] = apiVersion;
        
        await _next(context);
    }

    private string ExtractApiVersion(HttpRequest request)
    {
        // From Accept header: Accept: application/vnd.api.v1.2+json
        var acceptHeader = request.Headers.Accept.FirstOrDefault();
        if (acceptHeader?.Contains("vnd.api") == true)
        {
            var match = Regex.Match(acceptHeader, @"vnd\.api\.v(\d+)\.(\d+)");
            if (match.Success)
            {
                return $"{match.Groups[1].Value}.{match.Groups[2].Value}";
            }
        }

        // From custom header: X-API-Version: 1.2
        if (request.Headers.TryGetValue("X-API-Version", out var versionHeader))
        {
            return versionHeader.First();
        }

        // Default version
        return "1.0";
    }
}
```

### Controller-Based Versioning
```csharp
[ApiController]
[ApiVersion("1.0")]
[Route("api/v{version:apiVersion}/[controller]")]
public class ItemsV1Controller : ControllerBase
{
    [HttpGet]
    public async Task<ActionResult<ApiResponse<IEnumerable<ItemV1Dto>>>> GetItems()
    {
        // V1 implementation
        var items = await _service.GetItemsV1Async();
        return Ok(ApiResponse<IEnumerable<ItemV1Dto>>.Success(items));
    }
}

[ApiController]
[ApiVersion("2.0")]
[Route("api/v{version:apiVersion}/[controller]")]
public class ItemsV2Controller : ControllerBase
{
    [HttpGet]
    public async Task<ActionResult<ApiResponse<IEnumerable<ItemV2Dto>>>> GetItems(
        [FromQuery] ItemsFilterV2 filters)
    {
        // V2 implementation with enhanced filtering
        var items = await _service.GetItemsV2Async(filters);
        return Ok(ApiResponse<IEnumerable<ItemV2Dto>>.Success(items));
    }
}
```

## DTO Evolution Patterns

### Additive Changes (Minor Versions)
```csharp
// V1.0 - Original DTO
public record ItemV1Dto
{
    public int Id { get; init; }
    public string Name { get; init; } = string.Empty;
    public DateTime Date { get; init; }
    public decimal Price { get; init; }
}

// V1.1 - Add optional fields (backward compatible)
public record ItemV1_1Dto
{
    public int Id { get; init; }
    public string Name { get; init; } = string.Empty;
    public DateTime Date { get; init; }
    public decimal Price { get; init; }
    
    // New optional fields
    public string? Description { get; init; }
    public int AvailableSlots { get; init; }
    public bool IsActive { get; init; }
}
```

### Breaking Changes (Major Versions)
```csharp
// V1 - Original structure
public record ItemV1Dto
{
    public int Id { get; init; }
    public string Name { get; init; } = string.Empty;
    public DateTime Date { get; init; }
    public string StartLocation { get; init; } = string.Empty;
}

// V2 - Breaking changes (new major version required)
public record ItemV2Dto
{
    public int Id { get; init; }
    public string Name { get; init; } = string.Empty;
    public DateTime Date { get; init; }
    
    // Breaking: Split location into structured object
    public ItemLocationDto Location { get; init; } = new();
    
    // Breaking: Changed field name and type
    public ItemStatusDto Status { get; init; } = new();
}

public record ItemLocationDto
{
    public string StartPoint { get; init; } = string.Empty;
    public string? EndPoint { get; init; }
    public GeoCoordinateDto? Coordinates { get; init; }
}
```

## Swagger Documentation Strategy

### Multi-Version OpenAPI
```csharp
// Program.cs - Swagger configuration
builder.Services.AddSwaggerGen(options =>
{
    options.SwaggerDoc("v1", new OpenApiInfo 
    { 
        Title = "Project API V1", 
        Version = "v1.0",
        Description = "Project API Version 1 - Stable release"
    });
    
    options.SwaggerDoc("v2", new OpenApiInfo 
    { 
        Title = "Project API V2", 
        Version = "v2.0",
        Description = "Project API Version 2 - Enhanced features"
    });
    
    // Group endpoints by version
    options.DocInclusionPredicate((docName, description) =>
    {
        if (description.GroupName == null) return false;
        
        return docName switch
        {
            "v1" => description.GroupName.Contains("V1"),
            "v2" => description.GroupName.Contains("V2"),
            _ => false
        };
    });
});

// Configure swagger UI for multiple versions
app.UseSwaggerUI(options =>
{
    options.SwaggerEndpoint("/swagger/v1/swagger.json", "Project API V1");
    options.SwaggerEndpoint("/swagger/v2/swagger.json", "Project API V2");
    options.DisplayRequestDuration();
});
```

### Version-Specific Documentation
```csharp
public static class V1Endpoints
{
    public static void MapV1Endpoints(this IEndpointRouteBuilder app)
    {
        app.MapGet("/eventi", GetEventiV1)
           .WithName("GetEventi_V1")
           .WithTags("V1")
           .WithSummary("Get items list")
           .WithDescription("Returns all active items. Version 1 provides basic item information.")
           .Produces<ApiResponse<IEnumerable<ItemV1Dto>>>(200)
           .Produces(500);
    }
}

public static class V2Endpoints
{
    public static void MapV2Endpoints(this IEndpointRouteBuilder app)
    {
        app.MapGet("/items", GetItemsV2)
           .WithName("GetItems_V2")
           .WithTags("V2")
           .WithSummary("Get items list with advanced filtering")
           .WithDescription("Returns items with enhanced filtering and pagination. Version 2 adds location details and real-time availability.")
           .Produces<ApiResponse<PagedResult<ItemV2Dto>>>(200)
           .Produces(400)
           .Produces(500);
    }
}
```

## Backward Compatibility Strategies

### Response Transformation
```csharp
public class ApiVersionResponseTransformer
{
    public static object TransformResponse(object response, string apiVersion)
    {
        return apiVersion switch
        {
            "1.0" => TransformToV1(response),
            "1.1" => TransformToV1_1(response),
            "2.0" => response, // Latest version, no transformation needed
            _ => throw new NotSupportedException($"API version {apiVersion} not supported")
        };
    }

    private static object TransformToV1(object response)
    {
        if (response is ItemV2Dto itemV2)
        {
            return new ItemV1Dto
            {
                Id = itemV2.Id,
                Name = itemV2.Name,
                Date = itemV2.Date,
                StartLocation = itemV2.Location.StartPoint
            };
        }
        
        return response;
    }
}

// Usage in endpoint
public static async Task<IResult> GetItemById(
    int id, 
    IItemService service, 
    HttpContext context)
{
    var apiVersion = context.Items["ApiVersion"]?.ToString() ?? "1.0";
    var item = await service.GetItemByIdAsync(id);
    
    if (item == null)
        return Results.NotFound();
    
    var transformedResponse = ApiVersionResponseTransformer
        .TransformResponse(item, apiVersion);
    
    return Results.Ok(ApiResponse<object>.Success(transformedResponse));
}
```

### Request Adaptation
```csharp
public class CreateItemRequestAdapter
{
    public static CreateItemV2Request AdaptFromV1(CreateItemV1Request v1Request)
    {
        return new CreateItemV2Request
        {
            Name = v1Request.Name,
            Date = v1Request.Date,
            Location = new ItemLocationRequest
            {
                StartPoint = v1Request.StartLocation,
                EndPoint = null // V1 didn't have separate end location
            },
            Price = v1Request.Price,
            MaxCapacity = v1Request.MaxCapacity
        };
    }
}
```

## Deprecation Strategy

### Deprecation Headers
```csharp
public class DeprecationMiddleware
{
    private readonly RequestDelegate _next;
    private readonly Dictionary<string, DeprecationInfo> _deprecatedVersions;

    public DeprecationMiddleware(RequestDelegate next)
    {
        _next = next;
        _deprecatedVersions = new Dictionary<string, DeprecationInfo>
        {
            { "v1", new DeprecationInfo("2024-12-31", "v2", "Version 1 will be sunset on 2024-12-31. Please migrate to v2.") }
        };
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var version = ExtractVersionFromPath(context.Request.Path);
        
        if (_deprecatedVersions.TryGetValue(version, out var deprecationInfo))
        {
            context.Response.Headers.Add("Deprecation", $"date=\"{deprecationInfo.SunsetDate}\"");
            context.Response.Headers.Add("Sunset", deprecationInfo.SunsetDate);
            context.Response.Headers.Add("Link", $"</api/{deprecationInfo.MigrationTarget}>; rel=\"successor-version\"");
            
            // Optional: Add deprecation warning to response body
            if (context.Request.Headers.ContainsKey("X-Include-Deprecation-Info"))
            {
                context.Items["DeprecationWarning"] = deprecationInfo.Message;
            }
        }

        await _next(context);
    }

    private record DeprecationInfo(string SunsetDate, string MigrationTarget, string Message);
}
```

### Client Migration Guide
```csharp
public static class MigrationGuideGenerator
{
    public static object GenerateMigrationGuide(string fromVersion, string toVersion)
    {
        return (fromVersion, toVersion) switch
        {
            ("v1", "v2") => new
            {
                breaking_changes = new[]
                {
                    new { 
                        field = "start_location", 
                        change = "Split into location.startPoint and location.endPoint",
                        migration = "Use location.startPoint for the start location"
                    },
                    new {
                        field = "stato",
                        change = "Changed from string to structured object",
                        migration = "Use status.code for the status value"
                    }
                },
                new_features = new[]
                {
                    "Enhanced filtering with location-based search",
                    "Real-time availability updates",
                    "Pagination support for large result sets"
                },
                migration_timeline = new
                {
                    deprecation_date = "2024-06-01",
                    sunset_date = "2024-12-31",
                    recommended_migration_completion = "2024-10-31"
                }
            },
            _ => new { error = "Migration guide not available for this version combination" }
        };
    }
}

// Endpoint to get migration guide
app.MapGet("/api/migration-guide/{fromVersion}/to/{toVersion}", 
    (string fromVersion, string toVersion) =>
    {
        var guide = MigrationGuideGenerator.GenerateMigrationGuide(fromVersion, toVersion);
        return Results.Ok(ApiResponse<object>.Success(guide));
    })
    .WithTags("Migration")
    .WithSummary("Get migration guide between API versions");
```

## Testing Strategy

### Version-Specific Tests
```csharp
[TestFixture]
public class EventiEndpointVersionTests
{
    [Test]
    public async Task GetEventi_V1_ReturnsBasicEventInfo()
    {
        // Arrange
        var client = CreateTestClient();
        
        // Act
        var response = await client.GetAsync("/api/v1/eventi");
        
        // Assert
        response.StatusCode.Should().Be(HttpStatusCode.OK);
        var content = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ApiResponse<ItemV1Dto[]>>(content);
        
        result.Data.Should().NotBeNull();
        result.Data.First().Should().BeOfType<ItemV1Dto>();
    }

    [Test]
    public async Task GetItems_V2_ReturnsEnhancedItemInfo()
    {
        // Arrange
        var client = CreateTestClient();
        
        // Act
        var response = await client.GetAsync("/api/v2/eventi");
        
        // Assert
        response.StatusCode.Should().Be(HttpStatusCode.OK);
        var content = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ApiResponse<ItemV2Dto[]>>(content);
        
        result.Data.Should().NotBeNull();
        result.Data.First().Location.Should().NotBeNull();
        result.Data.First().Status.Should().NotBeNull();
    }
}
```

### Contract Testing
```csharp
[TestFixture]
public class ApiContractTests
{
    [Test]
    public async Task V1_Response_MaintainsBackwardCompatibility()
    {
        // Ensure V1 responses haven't changed structure
        var expectedSchema = LoadSchemaFromFile("item-v1-schema.json");
        var actualResponse = await GetV1ItemResponse();
        
        var isValid = JsonSchemaValidator.Validate(actualResponse, expectedSchema);
        isValid.Should().BeTrue("V1 API contract must remain stable");
    }
}
```

## Client SDK Versioning

### Version-Aware Client
```typescript
// TypeScript client example
export class ProjectApiClient {
    constructor(
        private baseUrl: string,
        private version: '1' | '2' = '2',
        private apiKey?: string
    ) {}

    async getItems(filters?: ItemsFilters): Promise<ItemDto[]> {
        const endpoint = `/api/v${this.version}/eventi`;
        
        if (this.version === '1') {
            return this.getEventiV1(filters as EventiFiltersV1);
        } else {
            return this.getEventiV2(filters as EventiFiltersV2);
        }
    }

    private async getItemsV1(filters?: ItemsFiltersV1): Promise<ItemV1Dto[]> {
        // V1 specific implementation
    }

    private async getItemsV2(filters?: ItemsFiltersV2): Promise<ItemV2Dto[]> {
        // V2 specific implementation
    }
}
```

## Best Practices Summary

### Do's
- ✅ **Use semantic versioning** for clear communication
- ✅ **Version only when necessary** - avoid version proliferation
- ✅ **Maintain backward compatibility** within major versions
- ✅ **Document breaking changes** clearly
- ✅ **Provide migration guides** and tooling
- ✅ **Test all supported versions** in CI/CD
- ✅ **Plan deprecation timeline** in advance

### Don'ts
- ❌ **Don't break existing clients** without major version bump
- ❌ **Don't version for internal changes** that don't affect API contract
- ❌ **Don't support too many versions** simultaneously (max 2-3)
- ❌ **Don't change URL structure** within same major version
- ❌ **Don't remove deprecated endpoints** without proper notice period
- ❌ **Don't version database schema** directly with API versions