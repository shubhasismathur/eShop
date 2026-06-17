# Dependency Map

This dependency map summarizes declared external dependencies for the eShop solution, using central package management in `Directory.Packages.props` and project-level package references.

## Dependencies

```mermaid
flowchart LR
    App["eShop Solution"]

    subgraph Web["Web Frameworks"]
        AspNet["ASP.NET Core 10.0.5"]
        Blazor["Blazor components 10.0.5"]
        ApiVersion["Asp.Versioning 10.0.0-preview.2"]
    end

    subgraph DB["Database and ORM"]
        EfCore["Entity Framework Core 10.0.5"]
        Npgsql["Npgsql EF Provider 10.0.1"]
        Dapper["Dapper 2.1.35"]
        Pgvector["Pgvector 0.3.2"]
    end

    subgraph Messaging["Messaging"]
        Rabbit["Aspire RabbitMQ 13.2.0"]
        Grpc["Grpc.AspNetCore 2.76.0"]
        Proto["Google.Protobuf 3.33.5"]
    end

    subgraph Cache["Caching"]
        Redis["Aspire StackExchange.Redis 13.2.0"]
    end

    subgraph Sec["Security"]
        Jwt["JwtBearer 10.0.5"]
        Oidc["OpenIdConnect 10.0.5"]
        Duende["Duende IdentityServer 7.3.2"]
        IdentityModel["IdentityModel 7.0.0"]
    end

    subgraph Obs["Observability"]
        OTel["OpenTelemetry 1.15.0"]
        Health["AspNetCore HealthChecks 9.0.0"]
    end

    subgraph Util["Utilities"]
        MediatR["MediatR 13.0.0"]
        Fluent["FluentValidation 12.0.0"]
        Scalar["Scalar.AspNetCore 2.8.6"]
    end

    App -->|"web"| Web
    App -->|"persistence"| DB
    App -->|"messaging"| Messaging
    App -->|"caching"| Cache
    App -->|"security"| Sec
    App -->|"observability"| Obs
    App -->|"utilities"| Util
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 3 | ASP.NET Core, Blazor, Asp.Versioning | API and UI runtime foundation |
| Database / ORM | 4 | EF Core, Npgsql, Dapper, Pgvector | PostgreSQL-centric persistence |
| Messaging | 3 | RabbitMQ integration, gRPC, Protobuf | Async events and gRPC communication |
| Caching | 1 | StackExchange.Redis integration | Basket state caching |
| Security | 4 | JwtBearer, OpenIdConnect, Duende IdentityServer | Centralized authn and token issuance |
| Observability | 2 | OpenTelemetry, HealthChecks | Distributed tracing and health endpoints |
| Utilities | 3 | MediatR, FluentValidation, Scalar | Cross-cutting app behavior |

### Version & Compatibility Risks

The repository targets .NET 9 while central package versions already track .NET 10 package lines (`10.0.5`), so runtime/tooling drift should be monitored. Duende IdentityServer and preview API versioning packages should be reviewed during platform upgrades for licensing and API compatibility.

### Notable Observations

- Central package management is enabled, which simplifies coordinated upgrades across many projects.
- The stack mixes EF Core and Dapper, indicating both ORM and hand-written query strategies.
- Messaging is split between RabbitMQ (events) and gRPC (basket service contract).
- OpenTelemetry packages are consistently applied, which eases migration observability baselining.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| MSTest | 4.0.2 | Unit and functional test support |
| xUnit v3 | 3.2.1 | Additional test framework usage |
| NSubstitute | 5.3.0 | Mocking and test doubles |
| Microsoft.AspNetCore.Mvc.Testing | 10.0.5 | API integration test host |
| Microsoft.AspNetCore.TestHost | 10.0.5 | In-memory integration tests |

Total test-scope dependencies: 5

The project has mature test infrastructure with both unit and functional capabilities, though some tests require optional workloads in this environment.
