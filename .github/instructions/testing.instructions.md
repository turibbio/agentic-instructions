---
applyTo:
  - "**/tests/**/*"
  - "**/*.test.[BACKEND_EXT]"
  - "**/*.test.[FRONTEND_EXT]"
  - "**/*.spec.[BACKEND_EXT]"
  - "**/*.spec.[FRONTEND_EXT]"
  - "backend/tests/**/*.[BACKEND_EXT]"
---

# Testing Instructions for [PROJECT_NAME]

## Testing Strategy

### Backend ([BACKEND_TECH])
Use **[BACKEND_TEST_FRAMEWORK]** for unit and integration tests:

```[BACKEND_LANGUAGE]
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
            [DateProperty] = [DateType].FromDateTime(DateTime.Now.AddDays(30)),
            [NumericProperty] = 1000
        };

        var expected[Entity] = new [Entity](1, request.[Property1], request.[DateProperty], "", request.[NumericProperty]);
        _mockRepository.Setup(r => r.AddAsync(It.IsAny<[Entity]>()))
                      .ReturnsAsync(expected[Entity]);

        // Act
        var result = await _[entity]Service.Create[Entity]Async(request);

        // Assert
        Assert.That(result.[Property1], Is.EqualTo("[Test Value]"));
        Assert.That(result.[NumericProperty], Is.EqualTo(1000));
        _mockRepository.Verify(r => r.AddAsync(It.IsAny<[Entity]>()), Times.Once);
    }

    [Test]
    public async Task Get[Entity]_ExistingId_Returns[Entity]()
    {
        // Arrange
        var [entityId] = 1;
        var expected[Entity] = new [Entity]([entityId], "[Test Name]", [DateType].FromDateTime(DateTime.Now), "[Description]", 100);
        _mockRepository.Setup(r => r.GetByIdAsync([entityId]))
                      .ReturnsAsync(expected[Entity]);

        // Act
        var result = await _[entity]Service.Get[Entity]Async([entityId]);

        // Assert
        Assert.That(result, Is.Not.Null);
        Assert.That(result.[Property1], Is.EqualTo("[Test Name]"));
        Assert.That(result.Id, Is.EqualTo([entityId]));
    }

    [Test]
    public async Task Get[Entity]_NonExistingId_ReturnsNull()
    {
        // Arrange
        var [entityId] = 999;
        _mockRepository.Setup(r => r.GetByIdAsync([entityId]))
                      .ReturnsAsync(([Entity])null);

        // Act
        var result = await _[entity]Service.Get[Entity]Async([entityId]);

        // Assert
        Assert.That(result, Is.Null);
    }
}
```

### Integration Tests
Test complete workflows with in-memory database:

```[BACKEND_LANGUAGE]
[TestFixture]
public class [Entity]IntegrationTests
{
    private [TEST_SERVER_TYPE] _factory;
    private HttpClient _client;

    [OneTimeSetUp]
    public void OneTimeSetup()
    {
        _factory = new [TEST_SERVER_TYPE]<Program>();
        _client = _factory.CreateClient();
    }

    [Test]
    public async Task Post[Entity]_ValidData_Returns201()
    {
        // Arrange
        var request = new Create[Entity]Request
        {
            [Property1] = "[Integration Test]",
            [DateProperty] = [DateType].FromDateTime(DateTime.Now.AddDays(15)),
            [NumericProperty] = 500
        };

        var json = JsonSerializer.Serialize(request);
        var content = new StringContent(json, Encoding.UTF8, "application/json");

        // Act
        var response = await _client.PostAsync("/api/[entities]", content);

        // Assert
        Assert.That(response.StatusCode, Is.EqualTo(HttpStatusCode.Created));
        
        var responseContent = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ApiResponse<[Entity]>>(responseContent);
        
        Assert.That(result.Success, Is.True);
        Assert.That(result.Data.[Property1], Is.EqualTo("[Integration Test]"));
    }

    [OneTimeTearDown]
    public void OneTimeTearDown()
    {
        _client?.Dispose();
        _factory?.Dispose();
    }
}
```

### Frontend ([FRONTEND_TECH])
Use **[FRONTEND_TEST_FRAMEWORK]** with [TESTING_LIBRARY]:

```[FRONTEND_LANGUAGE]
import { render, screen, fireEvent, waitFor } from '@testing-library/[FRONTEND_FRAMEWORK]';
import { describe, it, expect, vi } from '[FRONTEND_TEST_FRAMEWORK]';
import { [Entity]Card } from './[Entity]Card';

describe('[Entity]Card', () => {
  const mock[Entity] = {
    id: 1,
    [property1]: '[Test Entity]',
    [dateProperty]: new Date('[TEST_DATE]'),
    [property2]: '[Test Description]',
    [numericProperty]: 100
  };

  it('displays [entity] information correctly', () => {
    render(<[Entity]Card [entity]={mock[Entity]} [onAction]={() => {}} />);
    
    expect(screen.getByText('[Test Entity]')).toBeInTheDocument();
    expect(screen.getByText(/[TEST_DATE_FORMATTED]/)).toBeInTheDocument();
  });

  it('calls [onAction] when button is clicked', () => {
    const mock[OnAction] = vi.fn();
    
    render(<[Entity]Card [entity]={mock[Entity]} [onAction]={mock[OnAction]} />);
    
    const button = screen.getByRole('button', { name: /[ACTION_LABEL]/i });
    fireEvent.click(button);
    
    expect(mock[OnAction]).toHaveBeenCalledWith(1);
  });

  it('shows loading state when isLoading is true', () => {
    render(<[Entity]Card [entity]={mock[Entity]} [onAction]={() => {}} isLoading />);
    
    const button = screen.getByRole('button');
    expect(button).toBeDisabled();
    expect(screen.getByText('Loading...')).toBeInTheDocument();
  });
});
```

### Custom Hook Testing
Test complex logic in custom hooks:

```[FRONTEND_LANGUAGE]
import { renderHook, waitFor } from '@testing-library/[FRONTEND_FRAMEWORK]';
import { describe, it, expect, vi } from '[FRONTEND_TEST_FRAMEWORK]';
import { use[Entity]Data } from './use[Entity]Data';
import * as [entity]Service from '../services/[entity]Service';

vi.mock('../services/[entity]Service');

describe('use[Entity]Data', () => {
  const mock[entity]Service = vi.mocked([entity]Service);

  beforeEach(() => {
    vi.clearAllMocks();
  });

  it('loads [entity] data successfully', async () => {
    const mock[Entity] = {
      id: 1,
      [property1]: '[Test Entity]',
      [dateProperty]: new Date('[TEST_DATE]')
    };

    mock[entity]Service.get[Entity].mockResolvedValue(mock[Entity]);

    const { result } = renderHook(() => use[Entity]Data(1));

    expect(result.current.isLoading).toBe(true);
    expect(result.current.[entity]).toBe(null);

    await waitFor(() => {
      expect(result.current.isLoading).toBe(false);
    });

    expect(result.current.[entity]).toEqual(mock[Entity]);
    expect(result.current.error).toBe(null);
    expect(mock[entity]Service.get[Entity]).toHaveBeenCalledWith(1);
  });

  it('handles error when loading fails', async () => {
    mock[entity]Service.get[Entity].mockRejectedValue(new Error('[API Error]'));

    const { result } = renderHook(() => use[Entity]Data(1));

    await waitFor(() => {
      expect(result.current.isLoading).toBe(false);
    });

    expect(result.current.[entity]).toBe(null);
    expect(result.current.error).toBe('Error loading [entity]');
  });
});
```

## Testing Best Practices

### General Principles
- **AAA Pattern**: Arrange, Act, Assert (but don't use comments)
- **Single Responsibility**: Each test should verify one specific behavior
- **Independent Tests**: Tests should not depend on each other
- **Descriptive Names**: Test names should clearly describe what is being tested
- **Test Edge Cases**: Include boundary conditions and error scenarios

### Backend Testing Guidelines
- Use mocks for external dependencies (databases, HTTP clients, etc.)
- Test business logic separately from infrastructure concerns
- Include validation testing for all input scenarios
- Test exception handling and error conditions
- Use in-memory databases for integration tests when possible

### Frontend Testing Guidelines
- Test component behavior, not implementation details
- Mock external services and API calls
- Test user interactions and state changes
- Include accessibility testing (screen reader compatibility)
- Test responsive behavior and different screen sizes

### Performance Testing
- Include performance benchmarks for critical operations
- Test with realistic data volumes
- Monitor memory usage and potential leaks
- Test concurrent user scenarios for APIs

### Security Testing
- Test authentication and authorization scenarios
- Verify input validation and sanitization
- Test for common vulnerabilities (SQL injection, XSS, etc.)
- Include tests for security headers and HTTPS enforcement

## Testing Domain-Specific Logic

### Business Rules Testing
```[BACKEND_LANGUAGE]
[Test]
public void Validate[Action]_[Condition]_[ExpectedResult]()
{
    // Arrange
    var [entity] = new [Entity] { /* setup data */ };
    var request = new [Action]Request { /* setup request */ };

    // Act
    var result = [BusinessRulesClass].Validate[Action]([entity], request);

    // Assert
    Assert.That(result.IsValid, Is.[Expected]);
    if (!result.IsValid)
    {
        Assert.That(result.Errors, Contains.Item("[Expected error message]"));
    }
}
```

### API Endpoint Testing
```[BACKEND_LANGUAGE]
[Test]
public async Task Get[Entities]_WithPagination_ReturnsPagedResults()
{
    // Arrange
    var page = 1;
    var pageSize = 10;

    // Act
    var response = await _client.GetAsync($"/api/[entities]?page={page}&pageSize={pageSize}");

    // Assert
    Assert.That(response.StatusCode, Is.EqualTo(HttpStatusCode.OK));
    
    var content = await response.Content.ReadAsStringAsync();
    var result = JsonSerializer.Deserialize<PagedResponse<[Entity]>>(content);
    
    Assert.That(result.Data.Count(), Is.LessThanOrEqualTo(pageSize));
    Assert.That(result.Page, Is.EqualTo(page));
    Assert.That(result.PageSize, Is.EqualTo(pageSize));
}
```

### Form Validation Testing
```[FRONTEND_LANGUAGE]
it('validates required fields', async () => {
  render(<[Action]Form onSubmit={() => {}} />);
  
  const submitButton = screen.getByRole('button', { name: /submit/i });
  fireEvent.click(submitButton);
  
  await waitFor(() => {
    expect(screen.getByText('[Property1] is required')).toBeInTheDocument();
    expect(screen.getByText('Invalid email')).toBeInTheDocument();
  });
});
```

## Test Data Management

### Test Fixtures
Create reusable test data:

```[BACKEND_LANGUAGE]
public static class [Entity]TestData
{
    public static [Entity] Create[Entity](
        string [property1] = "[Default Name]",
        [DateType]? [dateProperty] = null,
        int [numericProperty] = 100)
    {
        return new [Entity](
            Id: 1,
            [Property1]: [property1],
            [DateProperty]: [dateProperty] ?? [DateType].FromDateTime(DateTime.Now.AddDays(30)),
            [Property2]: "[Default Description]",
            [NumericProperty]: [numericProperty]
        );
    }
    
    public static Create[Entity]Request CreateValid[Entity]Request()
    {
        return new Create[Entity]Request
        {
            [Property1] = "[Valid Test Name]",
            [DateProperty] = [DateType].FromDateTime(DateTime.Now.AddDays(15)),
            [NumericProperty] = 200
        };
    }
}
```

### Mock Setup Helpers
```[BACKEND_LANGUAGE]
public static class MockExtensions
{
    public static void Setup[Entity]Repository(
        this Mock<I[Entity]Repository> mock,
        IEnumerable<[Entity]> [entities])
    {
        mock.Setup(r => r.Get[Entities]Async())
            .ReturnsAsync([entities]);
            
        foreach (var [entity] in [entities])
        {
            mock.Setup(r => r.GetByIdAsync([entity].Id))
                .ReturnsAsync([entity]);
        }
    }
}
```

## Continuous Testing

### Test Automation
- Run tests automatically on every commit
- Include tests in CI/CD pipeline
- Generate code coverage reports
- Set minimum coverage thresholds

### Test Categories
- **Unit Tests**: Fast, isolated, test single units
- **Integration Tests**: Test component interactions
- **End-to-End Tests**: Test complete user workflows
- **Performance Tests**: Verify system performance under load
- **Security Tests**: Verify security requirements