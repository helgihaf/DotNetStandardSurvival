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
