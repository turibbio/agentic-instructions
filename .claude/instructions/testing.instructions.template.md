---
applyTo:
  - "**/tests/**/*"
  - "**/*.test.ts"
  - "**/*.test.tsx"
  - "**/*.spec.ts"
  - "**/*.spec.tsx"
  - "backend/tests/**/*.cs"
---

# Testing Instructions for [PROJECT_NAME]

## Testing Strategy

### Backend ([BACKEND_FRAMEWORK])
Use **[BACKEND_TEST_FRAMEWORK]** for unit and integration tests:

```csharp
[TestFixture]
public class [Entity]ServiceTests
{
    private I[Entity]Service _[entity]Service;
    private Mock<I[Entity]Repository> _mockRepository;

    [SetUp]
    public void Setup()
    {
        _mockRepository = new Mock<I[Entity]Repository>();
        _[entity]Service = new [Entity]Service(_mockRepository.Object);
    }

    [Test]
    public async Task Create[Entity]_ValidInput_Returns[Entity]()
    {
        // Arrange
        var request = new Create[Entity]Request
        {
            [Property1] = "[Test Value]",
            [Property2] = [TestDateValue],
            [Property3] = [TestNumericValue]
        };

        var expected[Entity] = new [Entity](1, request.[Property1], request.[Property2], "", request.[Property3]);
        _mockRepository.Setup(r => r.AddAsync(It.IsAny<[Entity]>()))
                      .ReturnsAsync(expected[Entity]);

        // Act
        var result = await _[entity]Service.Create[Entity]Async(request);

        // Assert
        Assert.That(result, Is.Not.Null);
        Assert.That(result.[Property1], Is.EqualTo(request.[Property1]));
        Assert.That(result.[Property3], Is.EqualTo(request.[Property3]));
    }

    [Test]
    public async Task Get[Entity]_NonExistent_ThrowsNotFoundException()
    {
        // Arrange
        _mockRepository.Setup(r => r.GetByIdAsync(999))
                      .ReturnsAsync(([Entity]?)null);

        // Act & Assert
        var ex = await Assert.ThrowsAsync<NotFoundException>(
            () => _[entity]Service.Get[Entity]Async(999));
        
        Assert.That(ex.Message, Does.Contain("[Entity] not found"));
    }
}
```

### Frontend ([FRONTEND_FRAMEWORK] + [FRONTEND_TEST_FRAMEWORK])
Use **[FRONTEND_TEST_FRAMEWORK]** with [TESTING_LIBRARY]:

```tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { describe, it, expect, vi, beforeEach } from 'vitest';
import { [Entity]Card } from '../[Entity]Card';
import { mock[Entity] } from '../../__mocks__/[entity].mock';

describe('[Entity]Card', () => {
  const defaultProps = {
    [entity]: mock[Entity],
    on[Action]: vi.fn(),
    isLoading: false
  };

  beforeEach(() => {
    vi.clearAllMocks();
  });

  it('should render [entity] information', () => {
    render(<[Entity]Card {...defaultProps} />);

    expect(screen.getByText('[Test Value]')).toBeInTheDocument();
    expect(screen.getByText(/[Test Date]/)).toBeInTheDocument();
    expect(screen.getByText(/[Test Measurement]/)).toBeInTheDocument();
  });

  it('should call on[Action] with [entity]Id', async () => {
    const mockOn[Action] = vi.fn();
    
    render(<[Entity]Card {...defaultProps} on[Action]={mockOn[Action]} />);
    
    fireEvent.click(screen.getByText('[Action Label]'));
    
    expect(mockOn[Action]).toHaveBeenCalledWith(mock[Entity].id);
  });

  it('should show loading state', () => {
    render(<[Entity]Card {...defaultProps} isLoading={true} />);
    
    expect(screen.getByText('[Loading Text]...')).toBeInTheDocument();
    expect(screen.getByRole('button')).toBeDisabled();
  });

  it('should handle [special state]', () => {
    const [specialEntity] = {
      ...mock[Entity],
      [specialProperty]: mock[Entity].[maxProperty]
    };

    render(<[Entity]Card {...defaultProps} [entity]={[specialEntity]} />);
    
    expect(screen.getByText('[Special State Label]')).toBeInTheDocument();
    expect(screen.getByRole('button')).toBeDisabled();
  });
});
```

## Common Testing Patterns

### API Mocking
```typescript
// __mocks__/[entity]Service.mock.ts
import { vi } from 'vitest';
import { [Entity]Service } from '../services/[entity]Service';

export const mock[Entity]Service = {
  get[Entities]: vi.fn(),
  get[Entity]: vi.fn(),
  create[Entity]: vi.fn(),
  [performAction]: vi.fn()
} as jest.Mocked<[Entity]Service>;

// Setup in test
vi.mock('../services/[entity]Service', () => ({
  [entity]Service: mock[Entity]Service
}));
```

### Testing Custom Hooks
```tsx
import { renderHook, act } from '@testing-library/react';
import { describe, it, expect, vi } from 'vitest';
import { use[Entity]Data } from '../use[Entity]Data';
import { mock[Entity]Service } from '../../__mocks__/[entity]Service.mock';

describe('use[Entity]Data', () => {
  it('should load [entity] data', async () => {
    const mock[Entity] = { id: 1, [property]: 'Test [Entity]' };
    mock[Entity]Service.get[Entity].mockResolvedValue(mock[Entity]);

    const { result } = renderHook(() => use[Entity]Data(1));

    expect(result.current.isLoading).toBe(true);

    await act(async () => {
      await new Promise(resolve => setTimeout(resolve, 0));
    });

    expect(result.current.isLoading).toBe(false);
    expect(result.current.[entity]).toEqual(mock[Entity]);
    expect(result.current.error).toBeNull();
  });

  it('should handle loading errors', async () => {
    mock[Entity]Service.get[Entity].mockRejectedValue(new Error('Network error'));

    const { result } = renderHook(() => use[Entity]Data(1));

    await act(async () => {
      await new Promise(resolve => setTimeout(resolve, 0));
    });

    expect(result.current.error).toBe('[Error message for loading failure]');
    expect(result.current.[entity]).toBeNull();
  });
});
```

### API Integration Tests
```csharp
[TestFixture]
public class [Entities]ControllerIntegrationTests
{
    private WebApplicationFactory<Program> _factory;
    private HttpClient _client;

    [SetUp]
    public void Setup()
    {
        _factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    // Use in-memory database for tests
                    services.AddDbContext<[ProjectName]DbContext>(options =>
                        options.UseInMemoryDatabase("TestDb"));
                });
            });

        _client = _factory.CreateClient();
    }

    [Test]
    public async Task GET_[Entities]_ReturnsSuccessAndCorrectContentType()
    {
        // Act
        var response = await _client.GetAsync("/api/[entities]");

        // Assert
        Assert.That(response.StatusCode, Is.EqualTo(HttpStatusCode.OK));
        Assert.That(response.Content.Headers.ContentType?.MediaType, 
                   Is.EqualTo("application/json"));
    }

    [Test]
    public async Task POST_[Entities]_ValidData_ReturnsCreated()
    {
        // Arrange
        var [entity]Request = new Create[Entity]Request
        {
            [Property1] = "Test [Entity]",
            [Property2] = [TestDateValue],
            [Property3] = [TestNumericValue]
        };

        var json = JsonSerializer.Serialize([entity]Request);
        var content = new StringContent(json, Encoding.UTF8, "application/json");

        // Act
        var response = await _client.PostAsync("/api/[entities]", content);

        // Assert
        Assert.That(response.StatusCode, Is.EqualTo(HttpStatusCode.Created));
        
        var responseContent = await response.Content.ReadAsStringAsync();
        var [entity] = JsonSerializer.Deserialize<[Entity]>(responseContent);
        Assert.That([entity]?.[Property1], Is.EqualTo([entity]Request.[Property1]));
    }

    [TearDown]
    public void TearDown()
    {
        _client?.Dispose();
        _factory?.Dispose();
    }
}
```

## Test Environment Configuration

### [FRONTEND_TEST_FRAMEWORK] Config
```typescript
// [test-config-file]
import { defineConfig } from '[test-framework]/config';
import [framework] from '@vitejs/plugin-[framework]';

export default defineConfig({
  plugins: [[framework]()],
  test: {
    environment: 'jsdom',
    setupFiles: ['./src/test/setup.ts'],
    globals: true
  }
});
```

### Setup File for [FRONTEND_FRAMEWORK] Testing
```typescript
// src/test/setup.ts
import '@testing-library/jest-dom';
import { vi } from 'vitest';

// Mock global functions
Object.defineProperty(window, 'matchMedia', {
  writable: true,
  value: vi.fn().mockImplementation(query => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: vi.fn(),
    removeListener: vi.fn(),
    addEventListener: vi.fn(),
    removeEventListener: vi.fn(),
    dispatchEvent: vi.fn(),
  })),
});
```

## Test Data and Fixtures

### Mock [Entity] Data
```typescript
// __mocks__/[entity].mock.ts
import { [Entity], [EntityType] } from '../types/[entity]';

export const mock[Entity]: [Entity] = {
  id: 1,
  [property1]: '[Test Value 1]',
  [property2]: '[Test Value 2]',
  [typeProperty]: '[type1]' as [EntityType],
  [dateProperty]: new Date('[TEST_DATE]'),
  [startDateProperty]: new Date('[TEST_START_DATE]'),
  [endDateProperty]: new Date('[TEST_END_DATE]'),
  [locationProperty]: '[Test Location]',
  [measurementProperty]: [TEST_MEASUREMENT],
  [maxProperty]: [TEST_MAX_VALUE],
  [countProperty]: [TEST_COUNT_VALUE],
  [priceProperty]: [TEST_PRICE_VALUE],
  [organizerProperty]: {
    id: 1,
    [organizerNameProperty]: '[Test Organization]',
    [organizerEmailProperty]: 'info@test[organization].com'
  },
  [statusProperty]: '[status1]'
};

export const mock[RelatedEntity] = {
  id: 1,
  [nameProperty]: '[Test Name]',
  [surnameProperty]: '[Test Surname]',
  [emailProperty]: '[test]@[domain].com',
  [birthProperty]: new Date('[TEST_BIRTH_DATE]'),
  [genderProperty]: 'M' as const,
  [identifierProperty]: [TEST_IDENTIFIER]
};
```

### Database Seeding for Tests
```csharp
public static class TestDataSeeder
{
    public static async Task Seed[Entity]Async([ProjectName]DbContext context)
    {
        if (!context.[Entities].Any())
        {
            var [entities] = new List<[Entity]>
            {
                new [Entity](1, "[Test Entity 1]", [TestDate1], "Test [entity] 1", [TestValue1]),
                new [Entity](2, "[Test Entity 2]", [TestDate2], "Test [entity] 2", [TestValue2])
            };

            context.[Entities].AddRange([entities]);
            await context.SaveChangesAsync();
        }
    }
}
```

## Best Practices Testing

### Test Naming Convention
```csharp
// Pattern: [MethodUnderTest]_[Scenario]_[ExpectedResult]
[Test]
public async Task Create[Entity]_ValidInput_ReturnsCreated[Entity]() { }

[Test]
public async Task Create[Entity]_[EntityWith[InvalidProperty]]_ThrowsValidationException() { }

[Test]
public async Task Get[Entity]_NonExistentId_ReturnsNull() { }
```

### Test Organization
```
tests/
├── unit/                    # Unit tests
│   ├── services/
│   ├── models/
│   └── utils/
├── integration/             # Integration tests
│   ├── api/
│   └── database/
└── e2e/                    # End-to-end tests (future)
    └── scenarios/
```

### Domain-Specific Assertions
```csharp
public static class [Entity]Assertions
{
    public static void ShouldBeValid[Entity](this [Entity] [entity])
    {
        Assert.That([entity].Id, Is.GreaterThan(0));
        Assert.That([entity].[Property1], Is.Not.Empty);
        Assert.That([entity].[DateProperty], Is.GreaterThan([TestDateComparison]));
        Assert.That([entity].[MaxProperty], Is.GreaterThan(0));
    }

    public static void ShouldHaveValid[Identifier](this [RelatedEntity] [relatedEntity])
    {
        Assert.That([relatedEntity].[IdentifierProperty], Is.GreaterThan(0));
        Assert.That([relatedEntity].[IdentifierProperty], Is.LessThan([MAX_IDENTIFIER_VALUE]));
    }
}
```

## Performance Testing

### Load Testing of APIs (future)
```csharp
[Test, Category("Performance")]
public async Task Get[Entities]_Under[TIME_LIMIT]ms_WhenCached()
{
    var stopwatch = Stopwatch.StartNew();
    
    var [entities] = await _[entity]Service.Get[Entities]Async();
    
    stopwatch.Stop();
    Assert.That(stopwatch.ElapsedMilliseconds, Is.LessThan([TIME_LIMIT]));
    Assert.That([entities].Count(), Is.GreaterThan(0));
}
```

## Common Test Commands

```bash
# Backend
cd backend
[backend-test-command]                              # All tests
[backend-test-command] --filter "Category=Unit"     # Only unit tests
[backend-test-command] --collect:"XPlat Code Coverage"  # With coverage

# Frontend
cd frontend
[frontend-test-command]                    # Test in watch mode
[frontend-test-coverage-command]       # With coverage report
[frontend-test-ci-command]            # Single run for CI
```