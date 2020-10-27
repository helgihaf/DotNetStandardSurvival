# The Options Pattern

The long read: https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-3.1

## Step 1
Create your options class. The following points present an opinionated way to do this, you don't have to follow any of them:
* Make it a public nested class the class that uses the options.
* Call it \<something\>Settings to differentiate it from the IOptions interface.
* Create a public constant that serves as a default name to use when reading from configuration sections.
```csharp
public class DeploymentRepository : ...
...
  public class Settings
  {
      public const string DeploymentRepository = nameof(DeploymentRepository);

      public string BaseUrl { get; set; }
      public string ApiKey { get; set; }
  }
```

## Step 2
Use it in your constructor. The example shows the most simple approach. There are multiple ways to do this, please read up on it.
```csharp
using Microsoft.Extensions.Options;

public class DeploymentRepository : ...
{
    private readonly Settings settings;
    
    ...
    
    public DeploymentRepository(IOptions<Settings> options)
    {
        settings = options?.Value ?? throw new ArgumentNullException(nameof(options));
    }
```

## Step 3
Set up DI so that your settings will be found when instantiating the main class:
```csharp
services.Configure<DeploymentRepository.Settings>(Configuration.GetSection(DeploymentRepository.Settings.DeploymentRepository));
```

If you need to construct your settings yourself, you can do it like this:
```csharp
DeploymentRepository DeploymentRepository(IServiceProvider serviceProvider)
{
  return new DeploymentRepository(Options.Create(new DeploymentRepository.Settings {...}));
}
```
