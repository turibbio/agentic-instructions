---
description: 'Advanced vali    private static bool DoesNotContainBannedWords(string name)
    {
        var bannedWords = new[] { "spam", "test", "fake" };
        return !bannedWords.Any(p => name.ToLower().Contains(p));on and error handling patterns for .NET Web APIs'
applyTo: 
  - "backend/**/*.cs"
  - "**/Program.cs"
---

# Validation and Error Handling

## Advanced FluentValidation

### Complex Validation Rules
```csharp
public class CreateItemRequestValidator : AbstractValidator<CreateItemRequest>
{
    public CreateItemRequestValidator()
    {
        RuleFor(x => x.Name)
            .NotEmpty().WithMessage("Name is required")
            .Length(3, 100).WithMessage("Name must be between 3 and 100 characters")
            .Must(DoesNotContainBannedWords).WithMessage("Name contains forbidden words");

        RuleFor(x => x.Date)
            .GreaterThan(DateOnly.FromDateTime(DateTime.Now.AddDays(7)))
            .WithMessage("Item must be scheduled at least 7 days in the future");

        RuleFor(x => x.MaxCapacity)
            .InclusiveBetween(1, 10000)
            .WithMessage("Number of participants must be between 1 and 10000");
    }

    private static bool DoesNotContainBannedWords(string name)
    {
        var bannedWords = new[] { "test", "fake", "spam" };
        return !bannedWords.Any(p => name.ToLower().Contains(p));
    }
}

// Registration in Program.cs
builder.Services.AddFluentValidationAutoValidation();
builder.Services.AddValidatorsFromAssemblyContaining<CreateItemRequestValidator>();
```

### Conditional Validation
```csharp
public class RegistrationRequestValidator : AbstractValidator<RegistrationRequest>
{
    public RegistrationRequestValidator()
    {
        RuleFor(x => x.Email)
            .NotEmpty()
            .EmailAddress();
            
        RuleFor(x => x.BirthDate)
            .NotEmpty()
            .Must(BeAtLeast18YearsOld)
            .WithMessage("Participant must be at least 18 years old");
            
        // Conditional validation
        When(x => x.RequiresCertificate, () =>
        {
            RuleFor(x => x.CertificatoMedico)
                .NotNull()
                .WithMessage("Medical certificate is required for this event");
        });
        
        // Complex business rule validation
        RuleFor(x => x)
            .MustAsync(BeUniqueParticipant)
            .WithMessage("Participant is already registered for this event");
    }
    
    private static bool BeAtLeast18YearsOld(DateOnly dataNascita)
    {
        var age = DateOnly.FromDateTime(DateTime.Now).Year - birthDate.Year;
        if (DateOnly.FromDateTime(DateTime.Now) < birthDate.AddYears(age))
            age--;
        return age >= 18;
    }
    
    private async Task<bool> BeUniqueParticipant(RegistrationRequest request, CancellationToken cancellationToken)
    {
        // Check database for existing registration
        // This would inject a service to check uniqueness
        return true; // Placeholder
    }
}
```

## Problem Details (RFC 7807)

### Configuration
```csharp
builder.Services.AddProblemDetails(options =>
{
    options.CustomizeProblemDetails = context =>
    {
        context.ProblemDetails.Extensions.Add("traceId", context.HttpContext.TraceIdentifier);
        context.ProblemDetails.Extensions.Add("instance", context.HttpContext.Request.Path);
        
        if (context.ProblemDetails.Status == 500)
        {
            context.ProblemDetails.Detail = "An internal error occurred. Contact support.";
        }
    };
});
```

### Custom Problem Details
```csharp
public static class ProblemDetailsExtensions
{
    public static ProblemDetails CreateValidationProblem(this IEnumerable<ValidationFailure> failures)
    {
        var problemDetails = new ValidationProblemDetails();
        
        foreach (var failure in failures)
        {
            if (problemDetails.Errors.ContainsKey(failure.PropertyName))
            {
                problemDetails.Errors[failure.PropertyName] = 
                    problemDetails.Errors[failure.PropertyName].Concat(new[] { failure.ErrorMessage }).ToArray();
            }
            else
            {
                problemDetails.Errors.Add(failure.PropertyName, new[] { failure.ErrorMessage });
            }
        }
        
        problemDetails.Title = "One or more validation errors occurred";
        problemDetails.Status = 400;
        
        return problemDetails;
    }
    
    public static ProblemDetails CreateBusinessRuleProblem(string title, string detail, string? instance = null)
    {
        return new ProblemDetails
        {
            Title = title,
            Detail = detail,
            Status = 422, // Unprocessable Entity
            Instance = instance
        };
    }
}
```

## Global Exception Handling

### Exception Handling Middleware
```csharp
public class GlobalExceptionHandlingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<GlobalExceptionHandlingMiddleware> _logger;

    public GlobalExceptionHandlingMiddleware(RequestDelegate next, ILogger<GlobalExceptionHandlingMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

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
            
            NotFoundException notFoundEx => new ProblemDetails
            {
                Title = "Resource not found",
                Status = 404,
                Detail = notFoundEx.Message,
                Instance = context.Request.Path
            },
            
            BusinessRuleException businessEx => new ProblemDetails
            {
                Title = "Business rule violation",
                Status = 422,
                Detail = businessEx.Message,
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

// Registration
app.UseMiddleware<GlobalExceptionHandlingMiddleware>();
```

## Custom Exceptions

### Domain-Specific Exceptions
```csharp
public abstract class ProjectException : Exception
{
    protected ProjectException(string message) : base(message) { }
    protected ProjectException(string message, Exception innerException) : base(message, innerException) { }
}

public class NotFoundException : ProjectException
{
    public NotFoundException(string entityName, object key) 
        : base($"{entityName} with key '{key}' was not found") { }
}

public class BusinessRuleException : ProjectException
{
    public BusinessRuleException(string message) : base(message) { }
}

public class EventFullException : BusinessRuleException
{
    public EventFullException(string eventName) 
        : base($"Event '{eventName}' has reached maximum capacity") { }
}

public class RegistrationClosedException : BusinessRuleException
{
    public RegistrationClosedException(string eventName) 
        : base($"Registration for event '{eventName}' is closed") { }
}
```

## Result Pattern Implementation

### Result Type
```csharp
public readonly struct Result<T>
{
    private readonly T? _value;
    private readonly string? _error;
    
    public bool IsSuccess { get; }
    public bool IsFailure => !IsSuccess;
    
    public T Value => IsSuccess ? _value! : throw new InvalidOperationException("Cannot access Value when Result is failure");
    public string Error => IsFailure ? _error! : throw new InvalidOperationException("Cannot access Error when Result is success");

    private Result(T value)
    {
        _value = value;
        _error = null;
        IsSuccess = true;
    }

    private Result(string error)
    {
        _value = default;
        _error = error;
        IsSuccess = false;
    }

    public static Result<T> Success(T value) => new(value);
    public static Result<T> Failure(string error) => new(error);
    
    public static implicit operator Result<T>(T value) => Success(value);
    public static implicit operator Result<T>(string error) => Failure(error);
}
```

### Service Implementation with Result Pattern
```csharp
public class ItemService : IItemService
{
    private readonly IItemRepository _repository;
    private readonly IValidator<CreateItemRequest> _validator;
    
    public async Task<Result<Item>> CreateItemAsync(CreateItemRequest request)
    {
        var validationResult = await _validator.ValidateAsync(request);
        if (!validationResult.IsValid)
        {
            var errors = string.Join(", ", validationResult.Errors.Select(e => e.ErrorMessage));
            return Result<Item>.Failure($"Validation failed: {errors}");
        }
        
        try
        {
            var item = await _repository.AddAsync(request.ToItem());
            return Result<Item>.Success(item);
        }
        catch (Exception ex)
        {
            return Result<Item>.Failure($"Failed to create item: {ex.Message}");
        }
    }
}
```

## Validation Extensions

### Custom Validation Attributes
```csharp
public class FutureDateAttribute : ValidationAttribute
{
    private readonly int _minimumDaysInFuture;
    
    public FutureDateAttribute(int minimumDaysInFuture = 0)
    {
        _minimumDaysInFuture = minimumDaysInFuture;
    }
    
    public override bool IsValid(object? value)
    {
        if (value is not DateOnly date)
            return false;
            
        var minimumDate = DateOnly.FromDateTime(DateTime.Now.AddDays(_minimumDaysInFuture));
        return date >= minimumDate;
    }
    
    public override string FormatErrorMessage(string name)
    {
        return $"{name} must be at least {_minimumDaysInFuture} days in the future";
    }
}

// Usage
public record CreateItemRequest
{
    [Required]
    [StringLength(100)]
    public string Name { get; init; } = string.Empty;
    
    [Required]
    [FutureDate(7)]
    public DateOnly Date { get; init; }
}
```

## API Error Response Standards

### Standardized Error Response Format
```csharp
public class ApiErrorResponse
{
    public string Title { get; set; } = string.Empty;
    public int Status { get; set; }
    public string Detail { get; set; } = string.Empty;
    public string? Instance { get; set; }
    public string TraceId { get; set; } = string.Empty;
    public Dictionary<string, string[]>? Errors { get; set; }
    public DateTime Timestamp { get; set; } = DateTime.UtcNow;
}
```

### Controller Error Handling
```csharp
[ApiController]
public class BaseController : ControllerBase
{
    protected ActionResult HandleResult<T>(Result<T> result)
    {
        if (result.IsSuccess)
        {
            return Ok(new { Success = true, Data = result.Value });
        }
        
        return BadRequest(new ApiErrorResponse
        {
            Title = "Operation failed",
            Status = 400,
            Detail = result.Error,
            Instance = HttpContext.Request.Path,
            TraceId = HttpContext.TraceIdentifier
        });
    }
    
    protected ActionResult HandleValidationResult(ValidationResult validationResult)
    {
        var errors = validationResult.Errors
            .GroupBy(e => e.PropertyName)
            .ToDictionary(
                g => g.Key,
                g => g.Select(e => e.ErrorMessage).ToArray()
            );
            
        return BadRequest(new ApiErrorResponse
        {
            Title = "Validation failed",
            Status = 400,
            Detail = "One or more validation errors occurred",
            Instance = HttpContext.Request.Path,
            TraceId = HttpContext.TraceIdentifier,
            Errors = errors
        });
    }
}