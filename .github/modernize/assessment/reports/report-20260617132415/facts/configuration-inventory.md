# Configuration & Externalized Settings Inventory

This inventory captures configuration sources and profile behavior across the eShop distributed solution, including appsettings files, launch profiles, Aspire composition, and environment-driven connection settings.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| AppHost settings | JSON | `src/eShop.AppHost/appsettings.json` | Host-level logging and optional OpenAI connection |
| Service settings | JSON | `src/*/appsettings.json` | Per-service defaults for logging, identity, event bus, and OpenAPI |
| Development overrides | JSON | `src/*/appsettings.Development.json` | Local DB and environment-specific overrides |
| Launch profiles | JSON | `src/*/Properties/launchSettings.json` | Local URLs, environment, and profile startup values |
| Central package config | MSBuild props | `Directory.Packages.props` | Centralized dependency version management |
| Build defaults | MSBuild props | `Directory.Build.props` | Shared build options for solution |
| Aspire composition | C# config | `src/eShop.AppHost/Program.cs` | Service wiring, references, waits, and external endpoints |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | `dotnet build` default local | Developer diagnostics and fast iteration | Standard SDK with debug symbols |
| Release | `dotnet build -c Release` | Optimized deployment build | Standard SDK with optimization |
| Launch profile `http` | IDE or `dotnet run --launch-profile http` | Local HTTP endpoint startup | Service-specific URL and env vars |
| Launch profile `https` | IDE or explicit launch profile | Local HTTPS endpoint startup | TLS endpoint plus same service config |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Development | `ASPNETCORE_ENVIRONMENT=Development` in launch settings | `appsettings.json` + `appsettings.Development.json` | Local PostgreSQL connection strings, local URLs |
| Default | Runtime default without explicit env | `appsettings.json` | Base logging and event bus settings |
| AppHost http/https | Launch profile selection in AppHost | AppHost launch settings + service references | Endpoint exposure and dashboard URLs |

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings:EventBus` | `amqp://localhost` | Default and development | API and processor appsettings |
| `ConnectionStrings:CatalogDB` | Localhost postgres connection | Development | Catalog.API development settings |
| `ConnectionStrings:OrderingDB` | Localhost postgres connection | Development | Ordering.API development settings |
| `ConnectionStrings:IdentityDB` | Localhost postgres connection | Development | Identity.API development settings |
| `ConnectionStrings:WebHooksDB` | Localhost postgres connection | Development | Webhooks.API development settings |
| `ConnectionStrings:Redis` | `localhost` | Default | Basket.API settings |
| `Identity:Audience` | service-specific value | Default | Basket, Ordering, Webhooks settings |
| `EventBus:SubscriptionClientName` | service-specific value | Default | Service appsettings |
| `CatalogOptions:UseCustomizationData` | `false` | Default | Catalog.API settings |
| `SessionCookieLifetimeMinutes` | `60` | Default | WebApp settings |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| ASP.NET Core APIs and processors | .NET runtime defaults via launch profiles | Not explicitly set in repo | 1 in local profile |
| Basket.API | HTTP/2 enabled through Kestrel endpoint defaults | Not explicitly set in repo | 1 in local profile |
| AppHost | Dashboard and resource service URLs configured in launch profile | Not explicitly set in repo | 1 host process |

## Startup Dependency Chain

1. `postgres`, `redis`, and `eventbus` infrastructure start first in AppHost.
2. `identity-api` starts with database reference and health check.
3. `catalog-api`, `ordering-api`, `basket-api`, and `webhooks-api` start with database and event bus dependencies.
4. `order-processor` waits for `eventbus` and `ordering-api` readiness before processing events.
5. `webapp`, `webhooksclient`, and `mobile-bff` start after backend references are available.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `ConnectionStrings:*DB` passwords in development files | Database credentials | Inline development values (weak sample password) |
| `ConnectionStrings:OpenAi` | API credential pair | Placeholder only in AppHost settings |
| `WebhookSubscription.Token` usage | Integration token | Request payload persisted by Webhooks service |

### Secrets Provisioning Workflow

In local development, secrets are mainly provided through appsettings and launch profiles. In distributed runs, Aspire references inject service connection information into dependent services. The repository includes placeholders for external AI credentials, but no full external secret manager integration is declared in configuration files.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `useOpenAI` | `false` | Hardcoded switch in AppHost `Program.cs` |
| `useOllama` | `false` | Hardcoded switch in AppHost `Program.cs` |
| `CatalogOptions:UseCustomizationData` | `false` | Catalog API appsettings |
| `UseCustomizationData` (Identity/Webhooks) | `false` | Service appsettings |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET target baseline | 9 (repo guidance) | README |
| .NET SDK in environment | 10.0.300 | `dotnet --version` |
| Aspire | 13.2.0 | `Directory.Packages.props` |
| ASP.NET Core packages | 10.0.5 | `Directory.Packages.props` |
| EF Core packages | 10.0.5 | `Directory.Packages.props` |
| Npgsql EF Provider | 10.0.1 | `Directory.Packages.props` |
| OpenTelemetry | 1.15.0 | `Directory.Packages.props` |
| Duende IdentityServer | 7.3.2 | `Directory.Packages.props` |
