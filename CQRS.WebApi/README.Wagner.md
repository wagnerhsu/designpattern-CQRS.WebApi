## 2020-08-13
### Add version information
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
- Modify Controller
```cs
private IMediator _mediator;

protected IMediator Mediator => _mediator ??= HttpContext.RequestServices.GetService<IMediator>();
```
- Add Features for corresponding Command or Query
```cs
public class CreateProductCommand : IRequest<int>
{
	// Properties
	// ...
	public class CreateProductCommandHandler : IRequestHandler<CreateProductCommand, int>
	{
		// DI related
		public async Task<int> Handle(CreateProductCommand command, CancellationToken cancellationToken)
		{}
	}
}

public class GetProductByIdQuery : IRequest<Product>
{
	// Properties
	// ...
	public class GetProductByIdQueryHandler(GetProductByIdQuery query, CancellationToken cancellationToken) {}
}
```
1. xxComand与xxCommandHandler成对出现
2. 如果xxCommand继承自`IRequest<int>`而xxCommandHandler继承自`IRequestHandler<xxCommand,int>`
- Use Unit to return void
```cs
public class CreateProductCommand : IRequest<Unit>
{
	public class CreateProductCommandHandler : IRequestHandler<CreateProductCommand, Unit>
	{
		public async Task<Unit> Handle(CreateProductCommand command, CancellationToken cancellationToken)
		{
			return Unit.Value;
		}
	}
}
```