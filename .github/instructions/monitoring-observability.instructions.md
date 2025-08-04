---
description: 'Comprehensive monitoring, logging, and observability patterns for .NET applications'
applyTo: 
  - "backend/**/*.cs"
  - "**/Program.cs"
  - "**/appsettings*.json"
---

# Monitoring and Observability - .NET Applications

## Structured Logging with Serilog

### Configuration
```csharp
// Program.cs
builder.Host.UseSerilog((context, configuration) =>
{
    configuration
        .ReadFrom.Configuration(context.Configuration)
        .Enrich.FromLogContext()
        .Enrich.WithProperty("ApplicationName", "Project.Api")
        .WriteTo.Console(outputTemplate: "[{Timestamp:HH:mm:ss} {Level:u3}] {Message:lj} {Properties:j}{NewLine}{Exception}")
        .WriteTo.File("logs/project-.log", 
            rollingInterval: RollingInterval.Day,
            retainedFileCountLimit: 7,
            outputTemplate: "{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz} [{Level:u3}] {Message:lj} {Properties:j}{NewLine}{Exception}");

    if (context.HostingEnvironment.IsProduction())
    {
        configuration.WriteTo.ApplicationInsights(
            context.Configuration.GetConnectionString("ApplicationInsights"),
            TelemetryConverter.Traces);
    }
});
```

### Usage Patterns
```csharp
public class ItemService
{
    private readonly ILogger<ItemService> _logger;
    
    public async Task<Item> CreateItemAsync(CreateItemRequest request)
    {
        _logger.LogInformation("Creating item {ItemName} for {MaxCapacity} participants", 
            request.Name, request.MaxCapacity);
            
        try
        {
            var item = await _repository.AddAsync(request);
            _logger.LogInformation("Item created successfully with ID {ItemId}", item.Id);
            return item;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to create item {ItemName}", request.Name);
            throw;
        }
    }
}
```

## Health Checks

### Configuration
```csharp
// Program.cs
builder.Services.AddHealthChecks()
    .AddNpgSql(builder.Configuration.GetConnectionString("DefaultConnection")!)
    .AddCheck("eventi_service", () =>
    {
        // Custom verification
        return HealthCheckResult.Healthy("Events service operational");
    });

// Endpoint
app.MapHealthChecks("/health", new HealthCheckOptions
{
    ResponseWriter = UIResponseWriter.WriteHealthCheckUIResponse
});

app.MapHealthChecks("/health/ready", new HealthCheckOptions
{
    Predicate = check => check.Tags.Contains("ready"),
    ResponseWriter = UIResponseWriter.WriteHealthCheckUIResponse
});
```

## Correlation ID Middleware

```csharp
public class CorrelationIdMiddleware
{
    private readonly RequestDelegate _next;
    private const string CorrelationIdHeader = "X-Correlation-ID";
    
    public CorrelationIdMiddleware(RequestDelegate next)
    {
        _next = next;
    }
    
    public async Task InvokeAsync(HttpContext context)
    {
        var correlationId = GetCorrelationId(context);
        
        context.Items["CorrelationId"] = correlationId;
        context.Response.Headers.Add(CorrelationIdHeader, correlationId);
        
        using (LogContext.PushProperty("CorrelationId", correlationId))
        {
            await _next(context);
        }
    }
    
    private static string GetCorrelationId(HttpContext context)
    {
        return context.Request.Headers[CorrelationIdHeader].FirstOrDefault() 
               ?? Guid.NewGuid().ToString();
    }
}

// Registration
app.UseMiddleware<CorrelationIdMiddleware>();
```

## Application Performance Monitoring

### OpenTelemetry Integration
```csharp
builder.Services.AddOpenTelemetry()
    .WithTracing(builder =>
    {
        builder
            .AddAspNetCoreInstrumentation()
            .AddHttpClientInstrumentation()
            .AddNpgsql()
            .AddConsoleExporter();
    })
    .WithMetrics(builder =>
    {
        builder
            .AddAspNetCoreInstrumentation()
            .AddHttpClientInstrumentation()
            .AddConsoleExporter();
    });
```

### Custom Metrics
```csharp
public class EventMetrics
{
    private readonly Counter<int> _eventsCreated;
    private readonly Histogram<double> _eventCreationDuration;
    
    public EventMetrics(IMeterFactory meterFactory)
    {
        var meter = meterFactory.Create("Project.Items");
        _eventsCreated = meter.CreateCounter<int>("events_created_total");
        _eventCreationDuration = meter.CreateHistogram<double>("event_creation_duration_seconds");
    }
    
    public void RecordEventCreated() => _eventsCreated.Add(1);
    public void RecordEventCreationDuration(double duration) => _eventCreationDuration.Record(duration);
}
```

## Error Tracking and Alerting

### Custom Exception Tracking
```csharp
public class ErrorTrackingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<ErrorTrackingMiddleware> _logger;
    
    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Unhandled exception occurred. TraceId: {TraceId}", 
                context.TraceIdentifier);
            
            // Send to error tracking service (e.g., Sentry, Application Insights)
            await RecordExceptionAsync(ex, context);
            throw;
        }
    }
    
    private async Task RecordExceptionAsync(Exception exception, HttpContext context)
    {
        // Implementation for external error tracking
    }
}
```

## Monitoring Best Practices

### Key Metrics to Track
- **Application Metrics**: Request rate, response time, error rate
- **Business Metrics**: Events created, registrations processed, participant counts
- **Infrastructure Metrics**: CPU, memory, disk usage
- **Database Metrics**: Connection pool usage, query duration

### Alerting Rules
```yaml
# Example alerting configuration
alerts:
  - name: high_error_rate
    condition: error_rate > 5%
    duration: 5m
    severity: critical
    
  - name: slow_response_time
    condition: avg_response_time > 2s
    duration: 10m
    severity: warning
    
  - name: database_connection_pool_exhausted
    condition: db_connection_pool_usage > 90%
    duration: 2m
    severity: critical
```

### Dashboard Configuration
Create dashboards that show:
- Request volume and latency trends
- Error rates by endpoint
- Database performance metrics
- Business KPIs (events, registrations)
- Infrastructure health

## Performance Monitoring

### Response Time Tracking
```csharp
public class PerformanceMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<PerformanceMiddleware> _logger;
    
    public async Task InvokeAsync(HttpContext context)
    {
        var stopwatch = Stopwatch.StartNew();
        
        await _next(context);
        
        stopwatch.Stop();
        
        if (stopwatch.ElapsedMilliseconds > 1000) // Log slow requests
        {
            _logger.LogWarning("Slow request detected: {Method} {Path} took {Duration}ms",
                context.Request.Method,
                context.Request.Path,
                stopwatch.ElapsedMilliseconds);
        }
    }
}
```

### Database Query Monitoring
```csharp
public class QueryPerformanceInterceptor : DbCommandInterceptor
{
    private readonly ILogger<QueryPerformanceInterceptor> _logger;
    
    public override ValueTask<InterceptionResult<DbDataReader>> ReaderExecutingAsync(
        DbCommand command, CommandEventData eventData, InterceptionResult<DbDataReader> result,
        CancellationToken cancellationToken = default)
    {
        var stopwatch = Stopwatch.StartNew();
        eventData.Context.Items["QueryStartTime"] = stopwatch;
        
        return base.ReaderExecutingAsync(command, eventData, result, cancellationToken);
    }
    
    public override ValueTask<DbDataReader> ReaderExecutedAsync(
        DbCommand command, CommandExecutedEventData eventData, DbDataReader result,
        CancellationToken cancellationToken = default)
    {
        if (eventData.Context.Items["QueryStartTime"] is Stopwatch stopwatch)
        {
            stopwatch.Stop();
            
            if (stopwatch.ElapsedMilliseconds > 1000)
            {
                _logger.LogWarning("Slow query detected: {Sql} took {Duration}ms",
                    command.CommandText, stopwatch.ElapsedMilliseconds);
            }
        }
        
        return base.ReaderExecutedAsync(command, eventData, result, cancellationToken);
    }
}
```