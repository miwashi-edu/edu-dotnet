# edu-dotnet

## Prerequisit

```bash
brew install --cask dotnet-sdk
```

## Instructions

```bash ~
cd 
cd ws
dotnet new webapi -n dotnet-backend
mkdir Controllers
```

## Controller

```bash
cat <<'EOF' > Controllers/HelloController.cs
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("[controller]")]
public class HelloController : ControllerBase
{
    [HttpGet]
    public string Get() => "Hello, world!";
}
EOF
```

## Server

```bash
cat <<'EOF' > Program.cs
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddCors(options =>
{
    options.AddDefaultPolicy(policy =>
        policy.AllowAnyOrigin().AllowAnyHeader().AllowAnyMethod());
});

var app = builder.Build();

app.UseCors();
app.UseAuthorization();
app.MapControllers();

app.Run();
EOF
```

## Run

```bash
dotnet run
```

## Visit

```bash
http://localhost:5081/hello
```
