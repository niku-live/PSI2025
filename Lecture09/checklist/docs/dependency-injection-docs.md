# Dependency Injection Implementation Guide

## 📚 **Official Microsoft Documentation**

### Primary Learning Resources
- [Dependency injection in .NET](https://docs.microsoft.com/en-us/dotnet/core/extensions/dependency-injection) - Complete guide to DI in .NET
- [Dependency injection in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection) - Web application DI patterns
- [.NET Generic Host](https://docs.microsoft.com/en-us/dotnet/core/extensions/generic-host) - Host builder and service configuration
- [Options pattern in .NET](https://docs.microsoft.com/en-us/dotnet/core/extensions/options) - Configuration object injection

### Service Lifetime Documentation
- [Service lifetimes](https://docs.microsoft.com/en-us/dotnet/core/extensions/dependency-injection#service-lifetimes) - Singleton, Scoped, Transient explained
- [Dependency injection guidelines](https://docs.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-guidelines) - Best practices and anti-patterns

## 🎓 **Learning Path: Beginner to Advanced**

### **Level 1: Fundamentals (Start Here)**
1. **Understanding IoC and DI Concepts**
   - Inversion of Control principle
   - Dependency Injection benefits
   - Constructor vs Property injection
   - Service lifetimes basics

2. **Basic Service Registration**
   ```csharp
   services.AddScoped<IUserService, UserService>();
   services.AddTransient<IValidator<User>, UserValidator>();
   services.AddSingleton<IMemoryCache, MemoryCache>();
   ```

3. **Simple Constructor Injection**
   ```csharp
   public class UserService
   {
       private readonly IUserRepository _repository;
       
       public UserService(IUserRepository repository)
       {
           _repository = repository ?? throw new ArgumentNullException(nameof(repository));
       }
   }
   ```

### **Level 2: Intermediate Patterns**
1. **Configuration Object Injection**
   ```csharp
   services.Configure<EmailOptions>(configuration.GetSection("Email"));
   services.AddSingleton<EmailOptions>(provider =>
       provider.GetRequiredService<IOptions<EmailOptions>>().Value);
   ```

2. **Factory Pattern Implementation**
   ```csharp
   services.AddSingleton<IPaymentProcessorFactory, PaymentProcessorFactory>();
   ```

3. **HttpClient Registration**
   ```csharp
   services.AddHttpClient<ApiService>(client =>
   {
       client.BaseAddress = new Uri("https://api.example.com");
       client.Timeout = TimeSpan.FromSeconds(30);
   });
   ```

### **Level 3: Advanced Techniques**
1. **Decorator Pattern with DI**
   ```csharp
   services.Decorate<IUserService, CachedUserService>();
   services.Decorate<IUserService, LoggingUserService>();
   ```

2. **Conditional Service Registration**
   ```csharp
   if (environment.IsDevelopment())
   {
       services.AddScoped<IEmailService, TestEmailService>();
   }
   else
   {
       services.AddScoped<IEmailService, SmtpEmailService>();
   }
   ```

3. **Custom Service Provider Extensions**
   ```csharp
   services.AddBusinessServices();
   services.AddDataAccess(connectionString);
   ```

## 🛠 **Implementation Guide**

### **Step 1: Define Service Interfaces**
```csharp
// Always start with clear abstractions
public interface IUserRepository
{
    Task<User> GetByIdAsync(int id);
    Task<IEnumerable<User>> GetActiveUsersAsync();
    Task SaveAsync(User user);
}

public interface IEmailService
{
    Task SendWelcomeEmailAsync(User user);
    Task SendNotificationAsync(User user, string message);
}

public interface IUserService
{
    Task<User> CreateUserAsync(CreateUserRequest request);
    Task<bool> DeactivateUserAsync(int userId);
}
```

### **Step 2: Implement Concrete Services**
```csharp
public class SqlUserRepository : IUserRepository
{
    private readonly string _connectionString;
    private readonly ILogger<SqlUserRepository> _logger;
    
    public SqlUserRepository(IConfiguration configuration, ILogger<SqlUserRepository> logger)
    {
        _connectionString = configuration.GetConnectionString("DefaultConnection");
        _logger = logger;
    }
    
    // Implementation details...
}

public class SmtpEmailService : IEmailService
{
    private readonly EmailConfiguration _config;
    private readonly ILogger<SmtpEmailService> _logger;
    
    public SmtpEmailService(EmailConfiguration config, ILogger<SmtpEmailService> logger)
    {
        _config = config;
        _logger = logger;
    }
    
    // Implementation details...
}
```

### **Step 3: Configure Services in Startup**
```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Configuration
    services.Configure<EmailConfiguration>(Configuration.GetSection("Email"));
    
    // Infrastructure
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
    
    // Application services
    services.AddScoped<IUserRepository, SqlUserRepository>();
    services.AddScoped<IEmailService, SmtpEmailService>();
    services.AddScoped<IUserService, UserService>();
    
    // Cross-cutting concerns
    services.AddMemoryCache();
    services.AddLogging();
    
    // HTTP clients
    services.AddHttpClient();
}
```

### **Step 4: Use Constructor Injection**
```csharp
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    private readonly IUserService _userService;
    private readonly ILogger<UsersController> _logger;
    
    public UsersController(IUserService userService, ILogger<UsersController> logger)
    {
        _userService = userService ?? throw new ArgumentNullException(nameof(userService));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }
    
    [HttpPost]
    public async Task<ActionResult<User>> CreateUser(CreateUserRequest request)
    {
        try
        {
            var user = await _userService.CreateUserAsync(request);
            return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to create user");
            return StatusCode(500, "Internal server error");
        }
    }
}
```

## ⚠️ **Common Pitfalls and How to Avoid Them**

### **1. Service Locator Anti-Pattern**
```csharp
// ❌ DON'T DO THIS
public class BadService
{
    private readonly IServiceProvider _serviceProvider;
    
    public BadService(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider; // Hides dependencies
    }
    
    public void DoWork()
    {
        var dependency = _serviceProvider.GetService<ISomeDependency>();
        // This makes testing difficult and hides actual dependencies
    }
}

// ✅ DO THIS INSTEAD
public class GoodService
{
    private readonly ISomeDependency _dependency;
    
    public GoodService(ISomeDependency dependency)
    {
        _dependency = dependency; // Clear dependency declaration
    }
}
```

### **2. Wrong Service Lifetimes**
```csharp
// ❌ INCORRECT LIFETIMES
services.AddSingleton<DbContext, AppDbContext>(); // Thread safety issues
services.AddTransient<ExpensiveApiClient>(); // Performance issues
services.AddSingleton<ICurrentUser, CurrentUser>(); // State sharing issues

// ✅ CORRECT LIFETIMES
services.AddScoped<DbContext, AppDbContext>(); // Per request
services.AddSingleton<IHttpClientFactory, HttpClientFactory>(); // Expensive to create
services.AddScoped<ICurrentUser, CurrentUser>(); // User-specific state
```

### **3. Too Many Dependencies (SRP Violation)**
```csharp
// ❌ TOO MANY DEPENDENCIES
public class OverloadedService
{
    public OverloadedService(
        IDep1 dep1, IDep2 dep2, IDep3 dep3, IDep4 dep4,
        IDep5 dep5, IDep6 dep6, IDep7 dep7, IDep8 dep8)
    {
        // This class probably has too many responsibilities
    }
}

// ✅ BREAK INTO SMALLER SERVICES
public class FocusedService
{
    public FocusedService(IDep1 dep1, IDep2 dep2)
    {
        // Single responsibility, fewer dependencies
    }
}
```

## 🧪 **Testing with Dependency Injection**

### **Unit Testing with Mocks**
```csharp
[Test]
public async Task CreateUser_ValidInput_ReturnsUser()
{
    // Arrange
    var mockRepository = new Mock<IUserRepository>();
    var mockEmailService = new Mock<IEmailService>();
    var mockLogger = new Mock<ILogger<UserService>>();
    
    mockRepository.Setup(r => r.SaveAsync(It.IsAny<User>()))
        .Returns(Task.CompletedTask);
    
    mockEmailService.Setup(e => e.SendWelcomeEmailAsync(It.IsAny<User>()))
        .Returns(Task.CompletedTask);
    
    var userService = new UserService(
        mockRepository.Object,
        mockEmailService.Object,
        mockLogger.Object);
    
    // Act
    var result = await userService.CreateUserAsync("test@example.com", "password");
    
    // Assert
    Assert.IsNotNull(result);
    Assert.AreEqual("test@example.com", result.Email);
    mockRepository.Verify(r => r.SaveAsync(It.IsAny<User>()), Times.Once);
    mockEmailService.Verify(e => e.SendWelcomeEmailAsync(It.IsAny<User>()), Times.Once);
}
```

### **Integration Testing with Test Host**
```csharp
public class IntegrationTestBase
{
    protected WebApplicationFactory<Program> Factory { get; private set; }
    
    [SetUp]
    public void SetUp()
    {
        Factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    // Replace real services with test doubles
                    services.RemoveAll<IEmailService>();
                    services.AddScoped<IEmailService, TestEmailService>();
                });
            });
    }
}

[Test]
public async Task CreateUser_Integration_Success()
{
    // Arrange
    var client = Factory.CreateClient();
    var request = new CreateUserRequest
    {
        Email = "test@example.com",
        Password = "password123"
    };
    
    // Act
    var response = await client.PostAsJsonAsync("/api/users", request);
    
    // Assert
    response.EnsureSuccessStatusCode();
    var user = await response.Content.ReadFromJsonAsync<User>();
    Assert.IsNotNull(user);
    Assert.AreEqual(request.Email, user.Email);
}
```

## 🔧 **Performance Considerations**

### **Service Lifetime Impact**
- **Singleton**: Created once, shared across all requests
  - ✅ Best for: Stateless services, expensive-to-create objects
  - ❌ Avoid for: Services with state, DbContext

- **Scoped**: One instance per request/scope
  - ✅ Best for: DbContext, repository patterns, user-specific services
  - ❌ Avoid for: Expensive-to-create stateless services

- **Transient**: New instance every time
  - ✅ Best for: Lightweight services, validators, mappers
  - ❌ Avoid for: Expensive services, services that hold resources

### **Memory and Performance Tips**
```csharp
// ✅ GOOD: Reuse expensive resources
services.AddSingleton<IConnectionStringProvider, ConnectionStringProvider>();
services.AddHttpClient<ApiClient>();

// ✅ GOOD: Scope database contexts
services.AddDbContext<AppDbContext>(options => 
    options.UseSqlServer(connectionString), ServiceLifetime.Scoped);

// ✅ GOOD: Use factories for conditional creation
services.AddSingleton<IPaymentProcessorFactory, PaymentProcessorFactory>();
```

## 🔍 **Debugging and Troubleshooting**

### **Common DI Container Errors**
1. **Service Not Registered**
   ```
   InvalidOperationException: Unable to resolve service for type 'IUserService'
   ```
   **Solution**: Register the service in ConfigureServices

2. **Circular Dependencies**
   ```
   InvalidOperationException: A circular dependency was detected
   ```
   **Solution**: Redesign to break the circular reference

3. **Captive Dependencies**
   ```
   Singleton service depends on Scoped service
   ```
   **Solution**: Match or decrease dependency lifetimes

### **Diagnostic Tools**
```csharp
// Enable detailed DI logging
services.AddLogging(builder =>
{
    builder.AddConsole();
    builder.SetMinimumLevel(LogLevel.Debug);
});

// Validate service registrations
services.AddOptions<ServiceProviderOptions>()
    .Configure(options => options.ValidateOnBuild = true);

// Use dependency validation extensions
services.ValidateDataAnnotations();
```

## 📋 **Implementation Checklist**

### **Design Phase**
- [ ] Identify service boundaries and responsibilities
- [ ] Define clear interface contracts
- [ ] Plan service lifetimes based on usage patterns
- [ ] Consider cross-cutting concerns (logging, caching, validation)

### **Implementation Phase**
- [ ] Create interfaces before implementations
- [ ] Validate constructor parameters for null values
- [ ] Use appropriate service lifetimes
- [ ] Register services in composition root
- [ ] Implement proper exception handling

### **Testing Phase**
- [ ] Write unit tests with mocked dependencies
- [ ] Create integration tests with test doubles
- [ ] Test service registration and resolution
- [ ] Verify proper cleanup of disposable services

## 🚀 **Advanced Topics**

### **Custom Service Registration Extensions**
```csharp
public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddBusinessServices(this IServiceCollection services)
    {
        services.AddScoped<IUserService, UserService>();
        services.AddScoped<IOrderService, OrderService>();
        services.AddScoped<IPaymentService, PaymentService>();
        return services;
    }
    
    public static IServiceCollection AddDataAccess(this IServiceCollection services, string connectionString)
    {
        services.AddDbContext<AppDbContext>(options =>
            options.UseSqlServer(connectionString));
        services.AddScoped<IUserRepository, SqlUserRepository>();
        services.AddScoped<IOrderRepository, SqlOrderRepository>();
        return services;
    }
}
```

### **Feature Flags with DI**
```csharp
services.AddSingleton<IFeatureManager, FeatureManager>();
services.AddScoped<IPaymentService>(provider =>
{
    var featureManager = provider.GetRequiredService<IFeatureManager>();
    return featureManager.IsEnabledAsync("NewPaymentSystem").Result
        ? provider.GetRequiredService<NewPaymentService>()
        : provider.GetRequiredService<LegacyPaymentService>();
});
```

## 📖 **Additional Resources**

- [Autofac Documentation](https://autofac.readthedocs.io/) - Popular third-party DI container
- [Simple Injector](https://simpleinjector.org/) - High-performance DI container
- [Castle Windsor](https://github.com/castleproject/Windsor) - Feature-rich DI container
- [Clean Architecture with .NET](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/common-web-application-architectures) - Architectural patterns
- [SOLID Principles](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles) - Design principles for maintainable code