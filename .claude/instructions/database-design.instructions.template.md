# Database Design Instructions - [PROJECT_NAME]

## [DATABASE_TECH] Design Principles

### Schema Strategy
- **Code-First Approach**: [ORM_TECH] migrations manage schema
- **Naming Convention**: [DB_NAMING_STYLE] for tables/columns, [CODE_NAMING_STYLE] for [BACKEND_LANGUAGE] entities
- **[DOMAIN_LANGUAGE] Domain Names**: Use [DOMAIN_LANGUAGE] for business entities ([core_entities])
- **Soft Deletes**: Use `[is_deleted_column]` boolean instead of hard deletes

### Table Design Patterns

#### Base Entity Pattern
```sql
-- All tables should include these base columns
CREATE TABLE [core_entities] (
    id SERIAL PRIMARY KEY,
    [created_at_column] TIMESTAMP NOT NULL DEFAULT NOW(),
    [updated_at_column] TIMESTAMP,
    [is_deleted_column] BOOLEAN NOT NULL DEFAULT FALSE,
    -- Entity-specific columns...
);
```

#### Audit Columns
```sql
-- For sensitive tables, add audit tracking
ALTER TABLE [core_entities] ADD COLUMN created_by_user_id INTEGER;
ALTER TABLE [core_entities] ADD COLUMN updated_by_user_id INTEGER;
ALTER TABLE [core_entities] ADD COLUMN version INTEGER NOT NULL DEFAULT 1;
```

### Core Domain Tables

#### [Core Entity 1] ([English Translation])
```sql
CREATE TABLE [core_entities] (
    id SERIAL PRIMARY KEY,
    [property1] VARCHAR(200) NOT NULL,
    [property2] TEXT,
    [type_property] VARCHAR(50) NOT NULL, -- '[type1]', '[type2]', '[type3]', etc.
    [date_property1] DATE NOT NULL,
    [date_property2] TIMESTAMP NOT NULL,
    [date_property3] TIMESTAMP NOT NULL,
    
    -- [Context Section 1]
    [location_property1] VARCHAR(300) NOT NULL,
    [location_property2] VARCHAR(300),
    [measurement_property1] DECIMAL(6,3) NOT NULL,
    [measurement_property2] INTEGER,
    
    -- [Context Section 2]
    [max_property] INTEGER NOT NULL,
    [price_property] DECIMAL(8,2) NOT NULL,
    
    -- [Context Section 3]
    [organizer_id_property] INTEGER NOT NULL,
    [status_property] VARCHAR(20) NOT NULL DEFAULT '[default_status]', -- '[status1]', '[status2]', '[status3]', '[status4]'
    [rules_property] VARCHAR(500),
    
    -- Base columns
    [created_at_column] TIMESTAMP NOT NULL DEFAULT NOW(),
    [updated_at_column] TIMESTAMP,
    [is_deleted_column] BOOLEAN NOT NULL DEFAULT FALSE,
    
    CONSTRAINT [fk_constraint_name] FOREIGN KEY ([organizer_id_property]) 
        REFERENCES [related_table](id),
    CONSTRAINT [chk_date_constraint] CHECK ([date_property1] > [date_property2]),
    CONSTRAINT [chk_date_range_constraint] CHECK ([date_property3] >= [date_property2])
);
```

## [ORM_TECH] Configuration

### DbContext Configuration
```csharp
public class [ProjectName]DbContext : DbContext
{
    public DbSet<[Entity1]> [Entities1] { get; set; }
    public DbSet<[Entity2]> [Entities2] { get; set; }
    public DbSet<[Entity3]> [Entities3] { get; set; }
    public DbSet<[Entity4]> [Entities4] { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Global query filters for soft deletes
        modelBuilder.Entity<[Entity1]>().HasQueryFilter(e => !e.[IsDeletedProperty]);
        modelBuilder.Entity<[Entity2]>().HasQueryFilter(p => !p.[IsDeletedProperty]);
        modelBuilder.Entity<[Entity3]>().HasQueryFilter(i => !i.[IsDeletedProperty]);

        // Apply configurations
        modelBuilder.ApplyConfigurationsFromAssembly(typeof([ProjectName]DbContext).Assembly);
    }

    public override async Task<int> SaveChangesAsync(CancellationToken cancellationToken = default)
    {
        // Auto-update timestamp fields
        var entries = ChangeTracker.Entries()
            .Where(e => e.Entity is BaseEntity && e.State is EntityState.Added or EntityState.Modified);

        foreach (var entry in entries)
        {
            var entity = (BaseEntity)entry.Entity;
            
            if (entry.State == EntityState.Added)
            {
                entity.[CreatedAtProperty] = DateTime.UtcNow;
            }
            else if (entry.State == EntityState.Modified)
            {
                entity.[UpdatedAtProperty] = DateTime.UtcNow;
            }
        }

        return await base.SaveChangesAsync(cancellationToken);
    }
}
```

### Performance Optimization
```csharp
// Connection pooling configuration
builder.Services.AddDbContext<[ProjectName]DbContext>(options =>
{
    options.Use[DatabaseProvider](connectionString, [providerOptions] =>
    {
        [providerOptions].EnableRetryOnFailure(
            maxRetryCount: 3,
            maxRetryDelay: TimeSpan.FromSeconds(30),
            errorCodesToAdd: null);
    });
}, ServiceLifetime.Scoped);

// Configure connection pooling
builder.Services.AddDbContextPool<[ProjectName]DbContext>(options =>
    options.Use[DatabaseProvider](connectionString), poolSize: 128);
```