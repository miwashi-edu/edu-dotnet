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

## Generate and install pgp in github

```bash
brew install gh
brew install gpg

gpg --batch --gen-key <<EOF
Key-Type: RSA
Key-Length: 4096
Name-Real: Service Bot
Name-Email: bot@example.com
Name-Comment: github-service-key
Expire-Date: 1y
%no-protection
%commit
EOF

KEY_ID=$(gpg --list-keys --with-colons "bot@example.com" | awk -F: '/^pub/ { print $5 }' | head -n1)
gpg --armor --export "$KEY_ID" > public.key
gh gpg-key add public.key --title "github-service-key"
rm public.key

echo "✅ PGP key $KEY_ID uploaded to GitHub."
```
