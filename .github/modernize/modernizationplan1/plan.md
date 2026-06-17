# Modernization Plan: modernizationplan1

**Project**: Basket.API

---

## Technical Framework

- **Language**: C# / .NET 10
- **Framework**: ASP.NET Core 10 with .NET Aspire service composition
- **Build Tool**: MSBuild / dotnet
- **Database**: Redis-backed basket store and RabbitMQ event bus integration
- **Key Dependencies**: Aspire.StackExchange.Redis, Grpc.AspNetCore,
  EventBusRabbitMQ, eShop.ServiceDefaults

---

## Overview

> This migration modernizes the Basket.API workload for Azure-hosted
> operation. The application currently depends on a locally provisioned Redis
> cache, local configuration files, and launch-profile environment settings
> for service wiring. The new architecture will:
>
> - move basket cache usage to an Azure-ready Redis service while preserving
>   existing basket behavior
> - externalize runtime settings needed in Azure so configuration is centrally
>   managed across environments
> - add a security remediation pass before later deployment activities
>
> The migration follows a phased approach that modernizes cache and
> configuration dependencies first and then validates dependency security.

---

## Migration Impact Summary

| Application | Original Service | New Azure Service | Authentication | Comments |
|-------------|------------------|-------------------|----------------|----------|
| Basket.API | Local Redis cache | Azure Cache for Redis | Managed identity | Modernize basket caching |
| Basket.API | Local appsettings/env | Azure App Configuration | Managed identity | Externalize runtime settings |
