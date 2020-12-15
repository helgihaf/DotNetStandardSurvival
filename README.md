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
        .AddLogging(configure => configure.AddConsole())
        .BuildServiceProvider();
    
    var mainAgent = new MainAgent(serviceProvider.GetRequiredService<ILogger<MainAgent>>());
    //...
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

## Configuration

### Install
```powershell
install-package Microsoft.Extensions.Configuration.Json
install-package Microsoft.Extensions.Configuration.Binder
```

### appsettings.json
Note: Set Copy to output directory = Always
```json
{
  "Message":  "Hello world",
  "CustomSection":  {
    "Customer": {
      "Id":  "007",
      "Name":  "Bond"
    }
  },
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  }
}
```

### Code
```c#
using Microsoft.Extensions.Configuration;
using System;

namespace ConsoleApp5
{
    class Program
    {
        static void Main(string[] args)
        {
            IConfiguration config = new ConfigurationBuilder()
                  .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
                  .Build();

            string message = config["Message"];
            
            // Note that the CustomSection is optional, it is simply shown here to demonstrate
            // how to navigate into sections.
            var customer = new Customer();
            config.GetSection("CustomSection:Customer").Bind(customer);     // You must install Microsoft.Extensions.Configuration.Binder for this to work

            var connectionString = config.GetConnectionString("BloggingDatabase");

            Console.WriteLine($"Message: {message}");
            Console.WriteLine($"Customer: {customer.Id}, {customer.Name}");
            Console.WriteLine($"Connection string: {connectionString}");

            Console.ReadKey();
        }
    }

    public class Customer
    {
        public string Id { get; set; }
        public string Name { get; set; }
    }
}
```
