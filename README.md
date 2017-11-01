# DotNetStandardSurvival

## Dependency Injection

### Install
```powershell
install-package Microsoft.Extensions.DependencyInjection
```

### Setup (console app)
```c#
static void Main(string[] args)
{
    var serviceProvider = new ServiceCollection()
                .AddTransient<IMyTransient, MyTransient>()
                .AddSingleton<IMySingleton, MySingleton>()
                .BuildServiceProvider();

    var t = serviceProvider.GetService<IMyTransient>();
}
```

### Setup (web app)
```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();

    services.AddTransient<IEmailSender, AuthMessageSender>();
    services.AddTransient<ISmsSender, AuthMessageSender>();
}
```

### Explicit Use
```c#
var bar = serviceProvider.GetService<IBarService>();
```

## Logging

### Install
```powershell
install-package Microsoft.Extensions.Logging
install-package Microsoft.Extensions.Logging.Console    # ...as needed
```

### Setup (console app)
```c#
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

static void Main(string[] args)
{
    var serviceProvider = new ServiceCollection()
        .AddLogging()
        .BuildServiceProvider();

    serviceProvider
        .GetService<ILoggerFactory>()
        .AddConsole(LogLevel.Debug);
}
```

### Use through DI
```c#
public class MainAgent
{
    private readonly ILogger logger;

    // Must specify <MainAgent> generic parameter for DI to work.
    public MainAgent(ILogger<MainAgent> logger)
    {
        this.logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }
    
    public void Run()
    {
        logger.LogInformation("Running like the wind...");
    }
}
```
