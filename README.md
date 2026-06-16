# dotnet-copilot-template

A template repository for dotnet applications that leverage GitHub Copilot (or other agents that respect an instructions file).

## Repository Structure

```
├── src/              # Application source code and projects
├── tests/            # Test projects (unit tests, integration tests)
├── adrs/             # Architecture Decision Records
├── .github/          # GitHub workflows and AI agent instructions
├── Directory.Build.props    # Shared MSBuild properties for all projects
├── Directory.Packages.props # Central Package Management configuration
└── Template.slnx            # Solution file
```

## Getting Started

### Prerequisites

- [.NET 10 SDK](https://dotnet.microsoft.com/download) or later

### Creating the Projects

You can use `dotnet new` or `aspire new` or similar CLI commands to build out the initial projects based on what kind of application you're building.

For example, the following will create a simple Aspire/AspNetApi/BlazorWebAssembly solution:

```powershell
$Solution = "Template.slnx"
$Root = "src"
$Prefix = "CompanyName.ProjectName"

dotnet new aspire-apphost        -n "$Prefix.AppHost"         -o "$Root/$Prefix.AppHost"
dotnet new aspire-servicedefaults -n "$Prefix.ServiceDefaults" -o "$Root/$Prefix.ServiceDefaults"
dotnet new web                   -n "$Prefix.Web"             -o "$Root/$Prefix.Web"
dotnet new blazorwasm            -n "$Prefix.BlazorClient"    -o "$Root/$Prefix.BlazorClient"
dotnet new classlib              -n "$Prefix.BlazorShared"    -o "$Root/$Prefix.BlazorShared"

dotnet sln $Solution add `
  "$Root/$Prefix.AppHost/$Prefix.AppHost.csproj" `
  "$Root/$Prefix.ServiceDefaults/$Prefix.ServiceDefaults.csproj" `
  "$Root/$Prefix.Web/$Prefix.Web.csproj" `
  "$Root/$Prefix.BlazorClient/$Prefix.BlazorClient.csproj" `
  "$Root/$Prefix.BlazorShared/$Prefix.BlazorShared.csproj"

dotnet add "$Root/$Prefix.Web/$Prefix.Web.csproj" reference `
  "$Root/$Prefix.ServiceDefaults/$Prefix.ServiceDefaults.csproj" `
  "$Root/$Prefix.BlazorShared/$Prefix.BlazorShared.csproj"

dotnet add "$Root/$Prefix.BlazorClient/$Prefix.BlazorClient.csproj" reference `
  "$Root/$Prefix.BlazorShared/$Prefix.BlazorShared.csproj"

dotnet add "$Root/$Prefix.AppHost/$Prefix.AppHost.csproj" reference `
  "$Root/$Prefix.Web/$Prefix.Web.csproj"

dotnet build $Solution
```

### Building the Solution

```bash
# Restore dependencies and build all projects
dotnet build

# Build in Release mode
dotnet build -c Release
```

### Running the Application

```bash
# Run the main application project
dotnet run --project src/YourProjectName/YourProjectName.csproj

# Run with specific environment
dotnet run --project src/YourProjectName/YourProjectName.csproj --environment Production
```

### Running Tests

```bash
# Run all tests
dotnet test

# Run tests with detailed output
dotnet test --verbosity normal

# Run tests with code coverage
dotnet test --collect:"XPlat Code Coverage"
```

## Configuration

Configuration settings are managed through:

- `appsettings.json` - Default configuration
- `appsettings.{Environment}.json` - Environment-specific overrides
- Environment variables - Runtime configuration
- User Secrets - Local development secrets (never committed)

## Development Guidelines

This repository follows conventions defined in [AGENTS.md](AGENTS.md), including:

- **Central Package Management**: All package versions managed in [Directory.Packages.props](Directory.Packages.props)
- **Code Style**: 2-space indentation, file-scoped namespaces, Allman-style braces
- **Testing**: xUnit framework with Arrange-Act-Assert pattern
- **Warnings as Errors**: All compiler warnings must be addressed

Run `dotnet format` to automatically format code according to project conventions.

## Contributing

1. Create a feature branch from `main`
2. Make your changes following the coding conventions
3. Ensure all tests pass (`dotnet test`)
4. Submit a pull request
