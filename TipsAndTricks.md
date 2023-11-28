# Tips and tricks

## Mocking an http connection

RichardSzalay.MockHttp
```csharp
_mockHttp = new MockHttpMessageHandler();
_mockHttp.When($"{BaseUrl}*").Respond(statusCode);
var myService = new MyService(_mockHttp.ToHttpClient(), _logger);
```
## Mocking HttpClient

```csharp
public class MyFakeHttpMessageHandler : HttpMessageHandler
...
var httpClient = new HttpClient(new MyFakeHttpMessageHandler(json, HttpStatusCode.OK)) { BaseAddress = new Uri("https://localhost") };
```

## Integration testing an Api
Use WebApplicationFactory<Program>, possibly public class MyIntegrationTests : IClassFixture<WebApplicationFactory<Program>>
```csharp
possibly HttpClient client = factory.CreateClient();
```

## Returning errors from an api
```csharp
return Problem(...);
```

## Nulls
Get around null in setup of tests:
```csharp
private IFixture _fixture = null!;
```

## Sourcelink
Make nuget packages F12-able: https://learn.microsoft.com/en-us/dotnet/standard/library-guidance/sourcelink
