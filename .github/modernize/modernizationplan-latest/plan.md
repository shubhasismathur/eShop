# Modernization Plan: eShop Azure Modernization

**Project**: eShop

---

## Technical Framework

- **Language**: C# / .NET 10
- **Framework**: ASP.NET Core 10.0 with .NET Aspire 13.2.0
- **Build Tool**: .NET SDK (dotnet)
- **Database**: PostgreSQL (via Aspire.Npgsql.EntityFrameworkCore.PostgreSQL 13.2.0)
- **Key Dependencies**: Entity Framework Core 10.0, gRPC 2.76.0, Duende IdentityServer 7.3.2, OpenTelemetry 1.15.0, StackExchange.Redis (via Aspire), RabbitMQ.Client (via Aspire), Azure OpenAI (optional)

---

## Overview

> This migration modernizes the eShop reference application by replacing locally hosted infrastructure services with managed Azure equivalents. The application currently runs Redis, PostgreSQL, and RabbitMQ as containerized local services orchestrated by .NET Aspire. The new architecture will:
>
> - Replace local Redis with Azure Cache for Redis to provide a fully managed, highly available distributed cache for basket storage
> - Replace local PostgreSQL instances with Azure Database for PostgreSQL Flexible Server to deliver managed relational database services for the Catalog, Identity, Ordering, and Webhooks APIs
> - Replace local RabbitMQ with Azure Service Bus to provide a fully managed, enterprise-grade messaging broker for event-driven communication across all microservices
> - Secure all Azure service connections using Managed Identity (passwordless authentication) to eliminate hardcoded credentials
> - Remediate known CVE vulnerabilities in project dependencies to ensure a secure deployment baseline
> - Deploy all containerized microservices to Azure Container Apps for a serverless, fully managed hosting environment
>
> The migration follows a phased approach: service-level code transformations first, followed by security scanning and remediation, and finally deployment to Azure Container Apps.

---

## Migration Impact Summary

| Application       | Original Service | New Azure Service                        | Authentication   | Comments                                                  |
|-------------------|------------------|------------------------------------------|------------------|-----------------------------------------------------------|
| Basket.API        | Redis            | Azure Cache for Redis                    | Managed Identity | Migrate basket distributed cache to Azure Cache for Redis |
| Catalog.API       | PostgreSQL       | Azure Database for PostgreSQL            | Managed Identity | Migrate catalog database to Azure Database for PostgreSQL |
| Identity.API      | PostgreSQL       | Azure Database for PostgreSQL            | Managed Identity | Migrate identity database to Azure Database for PostgreSQL|
| Ordering.API      | PostgreSQL       | Azure Database for PostgreSQL            | Managed Identity | Migrate ordering database to Azure Database for PostgreSQL|
| OrderProcessor    | PostgreSQL       | Azure Database for PostgreSQL            | Managed Identity | Migrate order processing DB to Azure Database for PostgreSQL|
| Webhooks.API      | PostgreSQL       | Azure Database for PostgreSQL            | Managed Identity | Migrate webhooks database to Azure Database for PostgreSQL|
| Basket.API        | RabbitMQ         | Azure Service Bus                        | Managed Identity | Migrate event bus messaging to Azure Service Bus          |
| Catalog.API       | RabbitMQ         | Azure Service Bus                        | Managed Identity | Migrate event bus messaging to Azure Service Bus          |
| Ordering.API      | RabbitMQ         | Azure Service Bus                        | Managed Identity | Migrate event bus messaging to Azure Service Bus          |
| OrderProcessor    | RabbitMQ         | Azure Service Bus                        | Managed Identity | Migrate event bus messaging to Azure Service Bus          |
| PaymentProcessor  | RabbitMQ         | Azure Service Bus                        | Managed Identity | Migrate event bus messaging to Azure Service Bus          |
| Webhooks.API      | RabbitMQ         | Azure Service Bus                        | Managed Identity | Migrate event bus messaging to Azure Service Bus          |
| WebApp            | RabbitMQ         | Azure Service Bus                        | Managed Identity | Migrate event bus messaging to Azure Service Bus          |
| All Services      | Local containers | Azure Container Apps                     | N/A              | Deploy all microservices to Azure Container Apps          |

---

## Open Questions & Questionnaire

- [x] Q: Should the plan include environment/infrastructure provisioning? → A: No — focus on code migration only (no infrastructure provisioning requested)
- [x] Q: Should the plan include integration testing? → A: No — skip integration testing (no environment provided or provisioned)
- [x] Q: Should the plan include a security scan and CVE remediation task? → A: Yes — include security/CVE remediation (default)
- [x] Q: Which Azure deployment target should the plan use? → A: Azure Container Apps (default)
