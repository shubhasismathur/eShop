# Architecture Diagram

eShop is a distributed .NET 10 solution orchestrated by .NET Aspire. It combines API services, background processors, and UI clients with shared service defaults.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        WebUI["WebApp"]
        WebhookUI["WebhookClient"]
        Mobile["ClientApp and HybridApp"]
    end

    subgraph App["Application Layer - ASP.NET Core Minimal APIs"]
        BasketApi["Basket API gRPC"]
        CatalogApi["Catalog API REST"]
        OrderingApi["Ordering API REST"]
        WebhooksApi["Webhooks API REST"]
        IdentityApi["Identity API"]
        OrderProc["Order Processor"]
        PaymentProc["Payment Processor"]
    end

    subgraph Data["Data Layer"]
        Pg[("PostgreSQL with pgvector")]
        Redis[("Redis")]
        Rabbit[("RabbitMQ")]
    end

    subgraph External["External Services"]
        OpenAI["OpenAI or Azure OpenAI optional"]
        Ollama["Ollama optional"]
    end

    WebUI -->|"HTTP and gRPC calls"| BasketApi
    WebUI -->|"HTTP"| CatalogApi
    WebUI -->|"HTTP"| OrderingApi
    WebhookUI -->|"HTTP"| WebhooksApi
    Mobile -->|"HTTP and gRPC"| CatalogApi
    BasketApi -->|"cache state"| Redis
    CatalogApi -->|"entity persistence"| Pg
    OrderingApi -->|"entity persistence"| Pg
    WebhooksApi -->|"entity persistence"| Pg
    App -->|"integration events"| Rabbit
    CatalogApi -->|"AI enrichment optional"| OpenAI
    CatalogApi -->|"local model optional"| Ollama
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Presentation | ASP.NET Core Blazor and Razor | .NET 10 | User-facing web and hybrid clients |
| API and Services | ASP.NET Core Minimal APIs and gRPC | .NET 10 | Business APIs and backend processing |
| Data Access | Entity Framework Core with Npgsql and StackExchange.Redis | EF Core 10 stack | Relational and cache persistence |
| Messaging | RabbitMQ via Aspire client packages | Current package-managed | Asynchronous integration events |
| Observability | OpenTelemetry and health checks | Current package-managed | Tracing, metrics, logs, and readiness |

### Data Storage & External Services

The system persists service data in PostgreSQL databases (`catalogdb`, `identitydb`, `orderingdb`, `webhooksdb`), stores shopping basket state in Redis, and uses RabbitMQ for integration events. Optional AI integrations route through OpenAI or Ollama when toggled in the AppHost.

### Key Architectural Decisions

- Uses .NET Aspire AppHost as the orchestration and service composition entry point.
- Centralizes cross-cutting defaults (service discovery, resilience, OpenTelemetry, health checks) in `eShop.ServiceDefaults`.
- Splits concerns into service-specific APIs plus asynchronous processors for order and payment flows.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation
        WebApp["WebApp"]
        WebhookClient["WebhookClient"]
        ClientApp["ClientApp and HybridApp"]
    end

    subgraph Business["Business Logic"]
        CatalogEndpoints["CatalogApi endpoints"]
        OrderingEndpoints["OrdersApi endpoints"]
        WebhooksEndpoints["WebHooksApi endpoints"]
        BasketGrpc["BasketService gRPC"]
        IdServer["IdentityServer"]
        OrderProcessor["OrderProcessor worker"]
        PaymentProcessor["PaymentProcessor worker"]
    end

    subgraph DataAccess["Data Access"]
        CatalogCtx["CatalogContext"]
        OrderingCtx["OrderingContext"]
        WebhooksCtx["WebhooksContext"]
        IdentityCtx["ApplicationDbContext"]
        BasketRepo["RedisBasketRepository"]
    end

    subgraph Infra["Infrastructure"]
        Health["MapHealthChecks"]
        Auth["JWT and authorization"]
        OTel["OpenTelemetry"]
        EventBus["EventBusRabbitMQ"]
    end

    WebApp -->|"calls"| CatalogEndpoints
    WebApp -->|"calls"| OrderingEndpoints
    WebhookClient -->|"calls"| WebhooksEndpoints
    ClientApp -->|"calls"| BasketGrpc
    CatalogEndpoints -->|"persist"| CatalogCtx
    OrderingEndpoints -->|"persist"| OrderingCtx
    WebhooksEndpoints -->|"persist"| WebhooksCtx
    IdServer -->|"persist"| IdentityCtx
    BasketGrpc -->|"store basket"| BasketRepo
    OrderProcessor -->|"consumes events"| EventBus
    PaymentProcessor -->|"consumes events"| EventBus
    Auth -.->|"guards"| OrderingEndpoints
    Auth -.->|"guards"| WebhooksEndpoints
    OTel -.->|"instruments"| Business
    Health -.->|"probes"| Business
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| CatalogApi | Presentation | Minimal API endpoints | Catalog search and item management |
| OrdersApi | Presentation | Minimal API endpoints | Order creation, retrieval, and state changes |
| WebHooksApi | Presentation | Minimal API endpoints | Webhook subscription management |
| BasketService | Business Logic | gRPC service | Basket read and write operations |
| OrderingContext | Data Access | EF Core DbContext | Ordering aggregate persistence |
| CatalogContext | Data Access | EF Core DbContext | Catalog aggregate persistence |
| RedisBasketRepository | Data Access | Repository | Basket state in Redis |
| EventBusRabbitMQ | Infrastructure | Messaging adapter | Publish and subscribe integration events |
