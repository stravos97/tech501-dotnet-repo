# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a .NET 8 ASP.NET Core Web API project called "Sparta Academy API" that manages Spartans, Courses, and Streams. The API provides CRUD operations with JWT authentication and includes Swagger documentation.

## Common Development Commands

### Build and Run
```bash
# Build the solution
dotnet build

# Run the main API project
dotnet run --project SpartaAcademyAPI/SpartaAcademyAPI.csproj

# Run with specific configuration
dotnet run --project SpartaAcademyAPI/SpartaAcademyAPI.csproj --configuration Release
```

### Testing
```bash
# Run all tests
dotnet test

# Run tests with coverage
dotnet test --collect:"XPlat Code Coverage"

# Run a specific test project
dotnet test SpartaAcademyAPI.Test/SpartaAcademyAPI.Test.csproj
```

### Database Operations
```bash
# Add a new migration
dotnet ef migrations add MigrationName --project SpartaAcademyAPI

# Update database
dotnet ef database update --project SpartaAcademyAPI

# Drop database
dotnet ef database drop --project SpartaAcademyAPI
```

### Docker Operations
```bash
# Build Docker image
docker build -t spartaacademyapi .

# Run container
docker run -d -p 8080:8080 -p 8081:8081 spartaacademyapi
```

## Architecture and Code Structure

### Key Architectural Patterns

**Repository Pattern**: The project uses a generic repository pattern with `ISpartaAcademyRepository<T>` interface and implementations like `SpartanRepository` and `CourseRepository`.

**Service Layer**: Business logic is encapsulated in service classes implementing `ISpartaAcademyService<T>`.

**DTO Pattern**: Data Transfer Objects are used for API responses, with AutoMapper handling entity-to-DTO conversions.

**Generic Design**: The repository and service layers use generics to support different entity types (Spartan, Course, Stream).

### Data Layer Architecture

- **DbContext**: `SpartaAcademyContext` manages the Entity Framework database context
- **In-Memory Database**: Configured to use Entity Framework In-Memory database by default
- **Entities**: `Spartan`, `Course`, and `Stream` models with navigation properties
- **Seeding**: `SeedData.cs` initializes database with sample data

### API Layer Architecture

- **Controllers**: RESTful controllers with JWT authentication (`[Authorize]` attribute)
- **DTOs**: Separate DTO classes for API responses with HATEOAS links
- **AutoMapper**: Configured in `AutoMapperProfile.cs` for entity-to-DTO mapping
- **Link Resolution**: `LinksResolver` utility for generating HATEOAS links

### Authentication & Authorization

- **JWT Authentication**: Configured in `JwtAuthenticationExtensions.cs`
- **Default Credentials**: username: "sparta", password: "global"
- **Token Endpoint**: `/api/auth/login` for obtaining bearer tokens
- **Protected Endpoints**: All CRUD operations require valid JWT tokens

### Key Dependencies

- **Entity Framework Core**: ORM with In-Memory database provider
- **AutoMapper**: Object-to-object mapping
- **JWT Bearer**: Authentication middleware
- **Swagger/OpenAPI**: API documentation
- **NUnit**: Testing framework with Moq for mocking

### Configuration Files

- **appsettings.json**: Application configuration
- **appsettings.Development.json**: Development-specific settings
- **launchSettings.json**: Development server configuration

## Testing Framework

The project uses NUnit testing framework with Moq for mocking. Test files are located in `SpartaAcademyAPI.Test/` project.

## API Documentation

- **Swagger UI**: Available at `/swagger` endpoint when running locally
- **Live Documentation**: https://spartaacademyapi20240530152521.azurewebsites.net/
- **Default Route**: Root URL redirects to Swagger documentation

## Deployment

- **Docker**: Dockerfile included for containerization
- **Azure**: Configured for Azure App Service deployment
- **Linux Deployment**: `deploy.sh` script for Linux server deployment with nginx reverse proxy