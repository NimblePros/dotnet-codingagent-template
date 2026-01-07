# Copilot Instructions

## Project Overview

This is a placeholder. Review the README.md and /src files for details about the repository purpose and structure and update this overview accordingly.

## Target Framework & Language

- **Framework**: .NET 10.0 (`net10.0`)
- **Language Version**: Latest C# features enabled
- **Nullable Reference Types**: Enabled project-wide
- **Implicit Usings**: Enabled

## Project Structure

```
├── src/              # Source code projects
├── tests/            # Test projects
├── Directory.Build.props    # Shared properties for all projects
├── Directory.Packages.props # Central Package Management (CPM) configuration
└── Template.slnx            # Solution file (XML format)
```

## Build Configuration

### Central Package Management

- Uses **MSBuild Central Package Management** (`ManagePackageVersionsCentrally=true`)
- All package versions are managed in [Directory.Packages.props](Directory.Packages.props)
- Individual projects reference packages WITHOUT version numbers in their `.csproj` files
- When adding a new package: Add `<PackageVersion Include="PackageName" Version="x.y.z" />` to [Directory.Packages.props](Directory.Packages.props)

### Compiler Settings

- **Treat Warnings As Errors**: Enabled (`TreatWarningsAsErrors=true`)
- **XML Documentation**: Warning 1591 (missing XML comments) is suppressed via `NoWarn`
- All projects automatically inherit settings from [Directory.Build.props](Directory.Build.props)

## Code Style Conventions

### File-Scoped Namespaces (Enforced)

```csharp
namespace MyProject.Feature;  // ✅ Use this (warning if not used)

// NOT this:
namespace MyProject.Feature   // ❌ Avoid
{
}
```

### Naming Conventions

- **Private fields**: Use underscore prefix with camelCase (`_myField`)
- **Constants**: Use PascalCase
- **Local variables**: Use `var` for built-in types and when type is apparent
- **Interfaces**: Prefix interfaces with `I` (e.g., `IRepository`)
- **Async Methods**: Suffix async methods with `Async` (e.g., `GetByIdAsync`)
- **Code Blocks**: Always use `{}` for blocks except single-line exits (e.g., `return`, `throw`)

### Code Formatting

- **Indentation**: 2 spaces (not tabs) for all `.cs` files
- **Line endings**: CRLF (Windows)
- **Encoding**: UTF-8 with BOM for code files
- **Braces**: Always use braces for control flow (`csharp_prefer_braces = true`)
- **Opening braces**: On new line (Allman style - `csharp_new_line_before_open_brace = all`)
- **Namespace directives**: File-scoped, outside namespace (enforced as warning)

### Modern C# Preferences

- **Primary constructors**: Preferred; ALWAYS assign to readonly fields
- **Top-level statements**: Preferred for program entry points
- **Pattern matching**: Prefer over `is` with cast and `as` with null check
- **Expression-bodied members**: Use for properties, indexers, accessors (but NOT for methods)

### Code Convention Enforcement

- Run `dotnet format` to auto-format code according to conventions
- Use analyzers from [Directory.Build.props](Directory.Build.props) to catch violations during build
- Consider using NSDepCop for architectural rules

### Configuration Management

- Use `appsettings.json` for default configuration
- Use `appsettings.{Environment}.json` for environment-specific overrides
- Use `IConfiguration` and `IOptions<T>` for accessing configuration in code
- Create strongly-typed settings classes for complex configurations
- Use `User Secrets` for sensitive data in development
- Never hardcode sensitive information in source code

## Logging

- Use `Microsoft.Extensions.Logging` for logging
- Log at appropriate levels: `Trace`, `Debug`, `Information`, `Warning`, `Error`, `Critical`
- Include relevant context in log messages
- Use Structured logging where possible
- Avoid logging sensitive information
- Include correlation IDs for tracing requests across services when possible

## Guard Clauses

- Use `Ardalis.GuardClauses` for input validation in non-UI code
- Prefer `FluentValidation` for complex validation scenarios, especially in UI layers
- Invalid inputs in UI layers should result in user-friendly error messages rather than exceptions
- Invalid inputs in non-UI layers should throw appropriate exceptions using guard clauses
- HTTP methods should return suitable status codes for non-success results (e.g., 400 Bad Request)

## Security

- Follow OWASP guidelines for secure coding practices
- Validate and sanitize all user inputs
- Use parameterized queries or ORM features to prevent SQL injection
- Implement proper authentication and authorization mechanisms
- Use HTTPS for all communications

## Coding Practices

- Write unit testable code wherever possible
- Offer to add tests to generated code if applicable - place such tests in the `tests/` folder
- Only test code that you control and that has business logic; do not test framework code
- Keep classes focused on a single responsibility
- Use Dependency Injection for managing dependencies
- Prefer composition over inheritance
- Use async/await for I/O-bound operations
- Keep methods short and focused; ideally under 20 lines
- Keep argument lists short; consider using parameter objects if more than 4 parameters are needed
- Prefer the `REPR` pattern for API design with FastEndpoints
- Use Domain-Driven Design (DDD) principles where applicable
- Use `*Request` and `*Response` suffixes for API layer messages
- Use `*Command` and `*Query` suffixes for use case layer messages
- Use DTOs for data transfer between layers; avoid exposing domain entities directly
- Use `*Details` and `*Summary` suffixes for DTOs as appropriate to indicate level of detail
- Avoid locating interfaces and their implementations in the same project

## Development Workflow

### Adding New Projects

When creating new projects in `src/` or `tests/`:

1. Production projects go in `src/`, test projects in `tests/`.
2. Test projects should use `xUnit`.

### Building & Running

```bash
# Build entire solution
dotnet build

# Run specific project
dotnet run --project src/YourProject/YourProject.csproj

# Run tests
dotnet test
```

### Testing Conventions

- Use `xUnit` for all tests
- Follow Arrange-Act-Assert pattern (but only with whitespace separation, no comments needed)
- Organize tests by feature in folders mirroring `src/` structure
- Do not name test classes with "Tests" suffix; do use "Tests" suffix for folder names if needed
  - Example: `ProductServiceTests/ProductService_Constructor.cs`
- Use `[Fact]` for simple tests, `[Theory]` with `[InlineData]` for parameterized tests
- Name test classes `ClassName_MethodName.cs`
- Name test methods `DoesSomething_GivenSomeCondition`
- Reading the full class name and method name should form a proper sentence describing the use case being tested.
  - Example `ProductService_Constructor._ReturnsInstanceGivenValidInputs`
- Separate Unit Tests and Integration Tests into different projects or folders as appropriate
- If mocking is needed, prefer NSubstitute over alternatives.
- Prefer Shouldly for assertions.
- Avoid commercially licensed testing libraries (e.g. FluentAssertions, Moq)

## Documentation Conventions

- Use Markdown for all documentation files
- Use headings (`#`, `##`, `###`) to organize content
- Include code snippets with proper syntax highlighting
- Run linting tools on documentation files to ensure consistency
- Lists and code listings should always have a blank line before and after them

## Key Files to Reference

- [.editorconfig](.editorconfig) - Complete code style rules and formatting preferences (151 lines)
- [Directory.Build.props](Directory.Build.props) - Shared build properties and compiler settings
- [Directory.Packages.props](Directory.Packages.props) - Centralized package version management
- [.gitignore](.gitignore) - Standard Visual Studio ignore patterns
