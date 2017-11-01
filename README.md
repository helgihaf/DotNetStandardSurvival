# DotNetStandardSurvival

## Dependency Injection

### Install

```
install-package Microsoft.Extensions.DependencyInjection
```
### Setup (console app)

```
static void Main(string[] args)
{
    var serviceProvider = new ServiceCollection()
                .AddTransient<IMyTransient, MyTransient>()
                .AddSingleton<IMySingleton, MySingleton>()
                .BuildServiceProvider();

    var agent = new MainAgent();
}
```

### Setup (web app)

```
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

```
var bar = serviceProvider.GetService<IBarService>();
```
