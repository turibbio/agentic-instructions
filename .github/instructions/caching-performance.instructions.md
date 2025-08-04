---
description: 'Caching strategies and performance optimization patterns for .NET applications'
applyTo: 
  - "backend/**/*.cs"
  - "**/Program.cs"
---

# Caching and Performance Optimization

## Response Caching

### Basic Configuration
```csharp
// Program.cs
builder.Services.AddResponseCaching();
app.UseResponseCaching();

// Implementation
itemsGroup.MapGet("/", async (IItemService itemService) =>
{
    var items = await itemService.GetItemsAsync();
    return Results.Ok(eventi);
})
.CacheOutput(policy => policy.Expire(TimeSpan.FromMinutes(5)));
```

### Advanced Output Caching
```csharp
builder.Services.AddOutputCache(options =>
{
    options.AddBasePolicy(builder => 
        builder.Expire(TimeSpan.FromMinutes(10)));
        
    options.AddPolicy("EventsPolicy", builder =>
        builder.Expire(TimeSpan.FromMinutes(5))
               .SetVaryByQuery("page", "pageSize"));
               
    options.AddPolicy("EventDetailsPolicy", builder =>
        builder.Expire(TimeSpan.FromHours(1))
               .SetVaryByRouteValue("id"));
});

app.UseOutputCache();

// Usage
app.MapGet("/api/eventi", GetEventiAsync)
   .CacheOutput("EventsPolicy");
```

## Distributed Caching

### Redis Configuration
```csharp
// Program.cs
builder.Services.AddStackExchangeRedisCache(options =>
{
    options.Configuration = builder.Configuration.GetConnectionString("Redis");
});

// Service with cache
public class CachedItemService : IItemService
{
    private readonly IItemService _baseService;
    private readonly IDistributedCache _cache;
    private readonly ILogger<CachedItemService> _logger;

    public async Task<IEnumerable<Item>> GetItemsAsync()
    {
        const string cacheKey = "eventi_list";
        
        var cachedEventi = await _cache.GetStringAsync(cacheKey);
        if (cachedEventi != null)
        {
            _logger.LogDebug("Events retrieved from cache");
            return JsonSerializer.Deserialize<IEnumerable<Item>>(cachedItems)!;
        }

        var eventi = await _baseService.GetEventiAsync();
        
        var serializedEventi = JsonSerializer.Serialize(eventi);
        await _cache.SetStringAsync(cacheKey, serializedEventi, 
            new DistributedCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5)
            });

        _logger.LogDebug("Events saved to cache");
        return eventi;
    }
}
```

### Memory Caching
```csharp
public class MemoryCachedItemService : IItemService
{
    private readonly IItemService _baseService;
    private readonly IMemoryCache _cache;
    private readonly ILogger<MemoryCachedItemService> _logger;

    public async Task<Item?> GetItemAsync(int id)
    {
        var cacheKey = $"item_{id}";
        
        if (_cache.TryGetValue(cacheKey, out Item? cachedItem))
        {
            _logger.LogInformation("Cache hit for item {ItemId}", id);
            return cachedItem;
        }
        
        var item = await _baseService.GetItemAsync(id);
```

## Pagination Helper

```csharp
public record PagedResult<T>(
    IEnumerable<T> Items,
    int TotalCount,
    int Page,
    int PageSize
) {
    public int TotalPages => (int)Math.Ceiling(TotalCount / (double)PageSize);
    public bool HasNextPage => Page < TotalPages;
    public bool HasPreviousPage => Page > 1;
}

public static class QueryableExtensions
{
    public static async Task<PagedResult<T>> ToPagedResultAsync<T>(
        this IQueryable<T> query,
        int page,
        int pageSize)
    {
        var totalCount = await query.CountAsync();
        var items = await query
            .Skip((page - 1) * pageSize)
            .Take(pageSize)
            .ToListAsync();
            
        return new PagedResult<T>(items, totalCount, page, pageSize);
    }
}
```

## Database Performance Optimization

### Query Optimization
```csharp
public class OptimizedItemRepository : IItemRepository
{
    private readonly ProjectDbContext _context;
    
    public async Task<IEnumerable<Item>> GetItemsWithParticipantsAsync()
    {
        return await _context.Items
            .Include(e => e.Participants)
            .AsNoTracking() // For read-only scenarios
            .ToListAsync();
    }
    
    public async Task<PagedResult<Item>> GetItemsPagedAsync(int page, int pageSize)
    {
        var query = _context.Items
            .AsNoTracking()
            .OrderBy(e => e.Date);
            
        return await query.ToPagedResultAsync(page, pageSize);
    }
    
    // Optimized count query
    public async Task<int> GetActiveItemsCountAsync()
    {
        return await _context.Items
            .Where(e => e.Date > DateOnly.FromDateTime(DateTime.Now))
            .CountAsync();
    }
}
```

### Connection Pooling
```csharp
// Program.cs
builder.Services.AddDbContext<ProjectDbContext>(options =>
{
    options.UseNpgsql(connectionString, npgsqlOptions =>
    {
        npgsqlOptions.CommandTimeout(30);
    });
}, ServiceLifetime.Scoped);

// Configure connection pooling
builder.Services.AddDbContextPool<ProjectDbContext>(options =>
{
    options.UseNpgsql(connectionString);
}, poolSize: 32);
```

## Async Patterns and Performance

### Efficient Async Operations
```csharp
public class PerformantItemService : IItemService
{
    private readonly IItemRepository _repository;
    private readonly IParticipantRepository _participantRepository;
    
    // Parallel processing
    public async Task<ItemSummary> GetItemSummaryAsync(int itemId)
    {
        var itemTask = _repository.GetByIdAsync(itemId);
        var participantsCountTask = _participantRepository.GetCountByItemAsync(itemId);
        var registrationsTask = GetRecentRegistrationsAsync(itemId);
        
        await Task.WhenAll(itemTask, participantsCountTask, registrationsTask);
        
        var item = await itemTask;
        var participantsCount = await participantsCountTask;
        var recentRegistrations = await registrationsTask;
        
        return new ItemSummary
        {
            Item = item,
            ParticipantsCount = participantsCount,
            RecentRegistrations = recentRegistrations
        };
    }
    
    // Batch operations
    public async Task ProcessMultipleItemsAsync(IEnumerable<int> itemIds)
    {
        const int batchSize = 10;
        var batches = itemIds.Chunk(batchSize);
        
        foreach (var batch in batches)
        {
            var tasks = batch.Select(ProcessSingleItemAsync);
            await Task.WhenAll(tasks);
        }
    }
}
```

## Rate Limiting

### Advanced Rate Limiting
```csharp
// Program.cs
builder.Services.AddRateLimiter(options =>
{
    // Global rate limiting
    options.GlobalLimiter = PartitionedRateLimiter.Create<HttpContext, string>(context =>
        RateLimitPartition.GetFixedWindowLimiter(
            partitionKey: context.User.Identity?.Name ?? context.Connection.RemoteIpAddress?.ToString(),
            factory: partition => new FixedWindowRateLimiterOptions
            {
                AutoReplenishment = true,
                PermitLimit = 100,
                Window = TimeSpan.FromMinutes(1)
            }));
    
    // Rate limiting for specific endpoints
    options.AddPolicy("EventCreation", context =>
        RateLimitPartition.GetSlidingWindowLimiter(
            partitionKey: context.User.Identity?.Name ?? "anonymous",
            factory: partition => new SlidingWindowRateLimiterOptions
            {
                AutoReplenishment = true,
                PermitLimit = 5,
                Window = TimeSpan.FromMinutes(1),
                SegmentsPerWindow = 4
            }));
});

app.UseRateLimiter();
```

## Performance Monitoring

### Performance Counters
```csharp
public class PerformanceMetrics
{
    private readonly Counter<int> _requestsProcessed;
    private readonly Histogram<double> _requestDuration;
    private readonly Counter<int> _cacheHits;
    private readonly Counter<int> _cacheMisses;
    
    public PerformanceMetrics(IMeterFactory meterFactory)
    {
        var meter = meterFactory.Create("Project.Performance");
        _requestsProcessed = meter.CreateCounter<int>("requests_processed_total");
        _requestDuration = meter.CreateHistogram<double>("request_duration_seconds");
        _cacheHits = meter.CreateCounter<int>("cache_hits_total");
        _cacheMisses = meter.CreateCounter<int>("cache_misses_total");
    }
    
    public void RecordRequest(double duration)
    {
        _requestsProcessed.Add(1);
        _requestDuration.Record(duration);
    }
    
    public void RecordCacheHit() => _cacheHits.Add(1);
    public void RecordCacheMiss() => _cacheMisses.Add(1);
}
```

## Best Practices

### Memory Management
- Use `IAsyncEnumerable<T>` for large data sets
- Implement proper disposal patterns
- Use object pooling for frequently allocated objects
- Monitor garbage collection patterns

### Database Optimization
- Use appropriate indexes
- Implement read replicas for read-heavy workloads
- Use database-specific optimization features
- Monitor query execution plans

### Caching Strategy
- Cache at multiple levels (memory, distributed, CDN)
- Implement cache invalidation strategies
- Use cache tags for complex invalidation scenarios
- Monitor cache hit rates and performance impact