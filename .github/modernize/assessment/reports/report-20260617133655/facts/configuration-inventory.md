# Configuration & Externalized Settings Inventory

eShop uses layered configuration across appsettings files, launch profiles, Aspire AppHost composition, and environment variable overrides. Sensitive runtime values are expected to come from environment-provided connection strings and service references.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| `appsettings.json` | .NET JSON config | `src/*/appsettings.json` | Base service defaults |
| `appsettings.Development.json` | .NET JSON config | `src/*/appsettings.Development.json` | Development overrides |
| `launchSettings.json` | Local profile config | `src/*/Properties/launchSettings.json` | Local run profiles and URLs |
| Aspire AppHost code | Orchestration config | `src/eShop.AppHost/Program.cs` | Service references, dependencies, and startup wiring |
| Environment variables | Externalized runtime config | host and container env | Includes endpoint and identity URL overrides |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | default local build | Developer diagnostics and local execution | Standard SDK pipeline |
| Release | CI or publish | Optimized deployment artifacts | Standard SDK pipeline |
| Multi-target MAUI | project target framework selection | Build mobile and hybrid variants | MAUI package set and workload requirements |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Development | `ASPNETCORE_ENVIRONMENT=Development` or launch profiles | `appsettings.Development.json` | Local endpoints, diagnostics, local service defaults |
| Default production-like | environment default in containers | `appsettings.json` + env vars | Connection strings and service discovery resolution |
| HTTP CI mode | `ESHOP_USE_HTTP_ENDPOINTS=1` in AppHost | AppHost runtime branch | Forces HTTP endpoints for E2E convenience |

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings:*` | service-provided | all | `appsettings*.json` plus Aspire references |
| `Identity__Url` | unset until injected | runtime | AppHost environment injection |
| `IdentityUrl` | unset until injected | runtime | AppHost environment injection for clients |
| `CallBackUrl` | unset until injected | runtime | AppHost endpoint self-reference |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | empty | optional | environment variable |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| ASP.NET APIs and workers | .NET runtime defaults | not hardcoded in repo | single instance per service by default |
| Redis, RabbitMQ, PostgreSQL containers | container image defaults | not hardcoded in repo | single persistent container each in AppHost |

## Startup Dependency Chain

1. `postgres`, `redis`, and `eventbus` containers initialize first.
2. `identity-api` starts with `identitydb` reference and health checks.
3. `basket-api`, `catalog-api`, `ordering-api`, and `webhooks-api` start after required data and event dependencies.
4. `order-processor` waits for `ordering-api` readiness and event bus.
5. `payment-processor` starts after event bus.
6. `webapp` and `webhooksclient` start after core APIs and identity references are available.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| PostgreSQL connection strings | Database credentials | Environment/service binding [MASKED] |
| Identity server signing and auth settings | Security secret material | Environment or secure configuration [MASKED] |
| Callback and endpoint URLs | Service integration secrets | Environment values [MASKED if sensitive] |

### Secrets Provisioning Workflow

Secrets are expected to be supplied by runtime environment and service discovery bindings rather than committed files. AppHost injects endpoint and identity URLs into dependent services, while connection strings are resolved through service references and environment overrides. No plaintext credential values are stored in the generated inventory artifacts.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `useOpenAI` | false | `src/eShop.AppHost/Program.cs` code toggle |
| `useOllama` | false | `src/eShop.AppHost/Program.cs` code toggle |
| `ESHOP_USE_HTTP_ENDPOINTS` | false | Environment variable |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET SDK and target | .NET 10 | `*.csproj` target frameworks |
| ASP.NET Core | .NET 10 stack | service project files |
| Entity Framework Core with Npgsql | package-managed current | API and infrastructure csproj files |
| OpenTelemetry | package-managed current | `eShop.ServiceDefaults.csproj` |
| RabbitMQ and Redis clients | Aspire package-managed current | API and AppHost csproj files |
