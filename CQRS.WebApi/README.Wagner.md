## 2020-08-13
### Use MediatR for AspNetCore
- Add reference
```powershell
dotnet add package MediatR
dotnet add package MediatR.Extensions.Microsoft.DependencyInjection
```
- Add MediatR service
```cs
services.AddScoped<IApplicationContext>(provider => provider.GetService<ApplicationContext>());
services.AddMediatR(Assembly.GetExecutingAssembly());
```