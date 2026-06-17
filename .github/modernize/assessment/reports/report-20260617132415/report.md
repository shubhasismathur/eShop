# Basket.API

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 12 |
| Mandatory Blockers | 1 |
| Potential Issues | 8 |

## Component Information

| Property | Value |
|----------|-------|
| Language | C# |
| Frameworks | net10.0 |
| Build tools | MSBuild |

## Cloud Readiness Issues

| Issue Name | Criticality | Story Points | Occurrences |
|------------|-------------|--------------|-------------|
| RabbitMQ usage is detected | Mandatory | 3 | [9](#RabbitMQ_usage_is_detected) |
| Local application configuration detected | Potential | 1 | [36](#Local_application_configuration_detected) |
| Hardcoded URLs detected | Potential | 1 | [11](#Hardcoded_URLs_detected) |
| Connection string is detected | Potential | 3 | [10](#Connection_string_is_detected) |
| Access to external resources via HTTP is detected | Potential | 3 | [6](#Access_to_external_resources_via_HTTP_is_detected) |
| Data caching is detected | Potential | 3 | [5](#Data_caching_is_detected) |
| Local or network IO operations detected | Potential | 3 | [2](#Local_or_network_IO_operations_detected) |
| Environment variables dependency detected | Potential | 3 | [1](#Environment_variables_dependency_detected) |
| Detected loading of dynamic assemblies. | Potential | 3 | [1](#Detected_loading_of_dynamic_assemblies) |
| Hardcoded sensitive data detected | Optional | 3 | [24](#Hardcoded_sensitive_data_detected) |
| Static content detected | Optional | 3 | [3](#Static_content_detected) |
| Synchronous API usage detected | Optional | 1 | [1](#Synchronous_API_usage_detected) |

### Issue Details

<details id="RabbitMQ_usage_is_detected">
<summary><b>RabbitMQ usage is detected</b> — affected files</summary>

- `src/EventBusRabbitMQ/RabbitMQEventBus.cs (line 234)`
- `src/EventBusRabbitMQ/RabbitMQEventBus.cs (line 28)`
- `src/EventBusRabbitMQ/RabbitMQEventBus.cs (line 26)`
- `src/EventBusRabbitMQ/RabbitMQEventBus.cs (line 133)`
- `src/EventBusRabbitMQ/RabbitMQEventBus.cs (line 79)`
- `src/EventBusRabbitMQ/RabbitMQEventBus.cs (line 39)`
- `src/EventBusRabbitMQ/RabbitMQEventBus.cs (line 74)`
- `src/EventBusRabbitMQ/RabbitMQEventBus.cs (line 74)`
- `src/EventBusRabbitMQ/RabbitMQEventBus.cs (line 76)`

</details>

<details id="Local_application_configuration_detected">
<summary><b>Local application configuration detected</b> — affected files</summary>

- `src/Basket.API/appsettings.json`
- `src/Basket.API/appsettings.json`
- `src/Basket.API/appsettings.json`
- `src/Basket.API/appsettings.json`
- `src/Catalog.API/appsettings.json`
- `src/Catalog.API/appsettings.json`
- `src/Catalog.API/appsettings.json`
- `src/Catalog.API/appsettings.json`
- `src/Catalog.API/appsettings.Development.json`
- `src/eShop.AppHost/appsettings.json`
- `src/Identity.API/appsettings.json`
- `src/Identity.API/appsettings.json`
- `src/Identity.API/appsettings.json`
- `src/Identity.API/appsettings.json`
- `src/Identity.API/appsettings.Development.json`
- `src/Ordering.API/appsettings.json`
- `src/Ordering.API/appsettings.json`
- `src/Ordering.API/appsettings.json`
- `src/Ordering.API/appsettings.json`
- `src/Ordering.API/appsettings.Development.json`
- `src/OrderProcessor/appsettings.json`
- `src/OrderProcessor/appsettings.json`
- `src/OrderProcessor/appsettings.json`
- `src/OrderProcessor/appsettings.Development.json`
- `src/PaymentProcessor/appsettings.json`
- `src/PaymentProcessor/appsettings.json`
- `src/PaymentProcessor/appsettings.json`
- `src/WebApp/appsettings.json`
- `src/WebApp/appsettings.json`
- `src/WebhookClient/appsettings.json`
- `src/Webhooks.API/appsettings.json`
- `src/Webhooks.API/appsettings.json`
- `src/Webhooks.API/appsettings.json`
- `src/Webhooks.API/appsettings.json`
- `src/Webhooks.API/appsettings.json`
- `src/Webhooks.API/appsettings.Development.json`

</details>

<details id="Hardcoded_URLs_detected">
<summary><b>Hardcoded URLs detected</b> — affected files</summary>

- `tests/ClientApp.UnitTests/Mocks/MockSettingsService.cs (line 24)`
- `tests/ClientApp.UnitTests/Mocks/MockSettingsService.cs (line 25)`
- `tests/ClientApp.UnitTests/Mocks/MockSettingsService.cs (line 26)`
- `src/eShop.AppHost/Extensions.cs (line 71)`
- `src/eShop.AppHost/Extensions.cs (line 77)`
- `src/eShop.ServiceDefaults/AuthenticationExtensions.cs (line 43)`
- `src/WebApp/Program.cs (line 31)`
- `src/WebApp/Extensions/Extensions.cs (line 30)`
- `src/WebApp/Extensions/Extensions.cs (line 33)`
- `src/WebApp/Extensions/Extensions.cs (line 37)`
- `src/WebhookClient/Extensions/Extensions.cs (line 18)`

</details>

<details id="Connection_string_is_detected">
<summary><b>Connection string is detected</b> — affected files</summary>

- `src/Basket.API/appsettings.json`
- `src/Basket.API/appsettings.json`
- `src/Catalog.API/appsettings.json`
- `src/Catalog.API/appsettings.Development.json`
- `src/Identity.API/appsettings.Development.json`
- `src/Ordering.API/appsettings.json`
- `src/Ordering.API/appsettings.Development.json`
- `src/PaymentProcessor/appsettings.json`
- `src/Webhooks.API/appsettings.json`
- `src/Webhooks.API/appsettings.Development.json`

</details>

<details id="Access_to_external_resources_via_HTTP_is_detected">
<summary><b>Access to external resources via HTTP is detected</b> — affected files</summary>

- `tests/Catalog.FunctionalTests/CatalogApiTests.cs (line 19)`
- `tests/Ordering.FunctionalTests/OrderingApiTests.cs (line 15)`
- `src/WebApp/Services/OrderingService.cs (line 2)`
- `src/WebAppComponents/Services/CatalogService.cs (line 6)`
- `src/WebhookClient/Services/WebHooksClient.cs (line 2)`
- `src/Webhooks.API/Services/WebhooksSender.cs (line 12)`

</details>

<details id="Data_caching_is_detected">
<summary><b>Data caching is detected</b> — affected files</summary>

- `src/Basket.API/Repositories/RedisBasketRepository.cs (line 5)`
- `src/Basket.API/Repositories/RedisBasketRepository.cs (line 7)`
- `src/Basket.API/Repositories/RedisBasketRepository.cs (line 12)`
- `src/Basket.API/Repositories/RedisBasketRepository.cs (line 15)`
- `src/Basket.API/Repositories/RedisBasketRepository.cs (line 24)`

</details>

<details id="Local_or_network_IO_operations_detected">
<summary><b>Local or network IO operations detected</b> — affected files</summary>

- `src/Catalog.API/Infrastructure/CatalogContextSeed.cs (line 25)`
- `src/Catalog.API/Apis/CatalogApi.cs (line 220)`

</details>

<details id="Environment_variables_dependency_detected">
<summary><b>Environment variables dependency detected</b> — affected files</summary>

- `src/Basket.API/Properties/launchSettings.json`

</details>

<details id="Detected_loading_of_dynamic_assemblies">
<summary><b>Detected loading of dynamic assemblies.</b> — affected files</summary>

- `src/IntegrationEventLogEF/Services/IntegrationEventLogService.cs (line 12)`

</details>

<details id="Hardcoded_sensitive_data_detected">
<summary><b>Hardcoded sensitive data detected</b> — affected files</summary>

- `src/Identity.API/Configuration/Config.cs (line 117)`
- `src/Identity.API/Configuration/Config.cs (line 51)`
- `src/Identity.API/Configuration/Config.cs (line 81)`
- `src/Identity.API/Data/Migrations/20230925223402_InitialMigration.Designer.cs (line 223)`
- `src/Identity.API/Data/Migrations/ApplicationDbContextModelSnapshot.cs (line 220)`
- `src/Identity.API/Quickstart/Account/AccountOptions.cs (line 14)`
- `src/Identity.API/Models/ManageViewModels/ChangePasswordViewModel.cs (line 16)`
- `src/Identity.API/Models/ManageViewModels/ChangePasswordViewModel.cs (line 6)`
- `src/Identity.API/Models/ManageViewModels/ChangePasswordViewModel.cs (line 12)`
- `src/Identity.API/Models/ManageViewModels/ChangePasswordViewModel.cs (line 17)`
- `src/Identity.API/Models/ManageViewModels/ChangePasswordViewModel.cs (line 17)`
- `src/Identity.API/Models/ManageViewModels/SetPasswordViewModel.cs (line 11)`
- `src/Identity.API/Models/ManageViewModels/SetPasswordViewModel.cs (line 7)`
- `src/Identity.API/Models/ManageViewModels/SetPasswordViewModel.cs (line 12)`
- `src/Identity.API/Models/ManageViewModels/SetPasswordViewModel.cs (line 12)`
- `src/Identity.API/Models/AccountViewModels/ResetPasswordViewModel.cs (line 14)`
- `src/Identity.API/Models/AccountViewModels/ResetPasswordViewModel.cs (line 15)`
- `src/Identity.API/Models/AccountViewModels/ResetPasswordViewModel.cs (line 15)`
- `src/Identity.API/Models/AccountViewModels/RegisterViewModel.cs (line 16)`
- `src/Identity.API/Models/AccountViewModels/RegisterViewModel.cs (line 12)`
- `src/Identity.API/Models/AccountViewModels/RegisterViewModel.cs (line 17)`
- `src/Identity.API/Models/AccountViewModels/RegisterViewModel.cs (line 17)`
- `src/WebApp/Extensions/Extensions.cs (line 77)`
- `src/WebhookClient/Extensions/Extensions.cs (line 53)`

</details>

<details id="Static_content_detected">
<summary><b>Static content detected</b> — affected files</summary>

- `src/Identity.API/Identity.API.csproj`
- `src/WebApp/WebApp.csproj`
- `src/WebhookClient/WebhookClient.csproj`

</details>

<details id="Synchronous_API_usage_detected">
<summary><b>Synchronous API usage detected</b> — affected files</summary>

- `src/Catalog.API/Infrastructure/CatalogContextSeed.cs (line 25)`

</details>

---

## Codebase Insights

> **Note:** These documents are generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

1. **[Architecture Diagram](facts/architecture-diagram.md)** — Understand the big picture: system layers and component relationships
2. **[Dependency Map](facts/dependency-map.md)** — Know what the project depends on and where the risks are
3. **[API & Service Contracts](facts/api-service-contracts.md)** — See how services communicate and what contracts they expose
4. **[Data Architecture](facts/data-architecture.md)** — Explore data models, storage, and data flow patterns
5. **[Configuration Inventory](facts/configuration-inventory.md)** — Review how the application is configured across environments
6. **[Business Workflows](facts/business-workflows.md)** — Trace end-to-end business processes and domain logic

[Share feedback](https://aka.ms/ghcp-appmod/feedback)
