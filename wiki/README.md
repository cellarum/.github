# Cellarum project wiki

Cellarum connects operational inventory through one API and shared record model.
This repository owns the [platform architecture and delivery plan](../ARCHITECTURE.md),
organization profile and brand assets. Product implementation and development
guidance live with each component.

## Repository guides

| Component | Wiki |
| --- | --- |
| API | [cellarum-api](https://github.com/cellarum/cellarum-api/tree/main/wiki) |
| Web app | [cellarum-app-web](https://github.com/cellarum/cellarum-app-web/tree/main/wiki) |
| MCP server | [cellarum-mcp](https://github.com/cellarum/cellarum-mcp/tree/main/wiki) |
| Swift client | [cellarum-app-swift](https://github.com/cellarum/cellarum-app-swift/tree/main/wiki) |
| Windows client | [cellarum-app-csharp](https://github.com/cellarum/cellarum-app-csharp/tree/main/wiki) |
| AWS Terraform | [terraform-aws-cellarum](https://github.com/cellarum/terraform-aws-cellarum/tree/main/wiki) |
| Azure Terraform | [terraform-azurerm-cellarum](https://github.com/cellarum/terraform-azurerm-cellarum/tree/main/wiki) |
| Self-hosted Docker | [selfhosted-docker-cellarum](https://github.com/cellarum/selfhosted-docker-cellarum/tree/main/wiki) |

Repository links follow the access rules of the corresponding project. A component's current source and acceptance checks establish implemented behavior; roadmap descriptions remain plans until delivered.

## Product architecture

The API owns records, Domain authorization and transactional event history. Web, native and MCP clients use that contract. Infrastructure repositories provide deployment components and the Docker stack composes application services for self-hosting.

The [organization profile](../profile/README.md) introduces the product. [Brand assets](../branding) contain the shared marks. Keep brand files here and link them from component documentation instead of creating competing copies.

## Documentation conventions

Use the [architecture vocabulary](../ARCHITECTURE.md#2-four-concepts): modules own
business capabilities; products assemble experiences; collections organize
navigation. Workspace is proposed vocabulary for the existing Domain permission
boundary, not an approved API/database rename. Distinguish those concepts from
persisted classes. Describe implemented behavior in the present tense; link issues
and roadmap pages for planned work.

Each repository's wiki is versioned under its wiki directory and reviewed with the source. Update the existing authoritative page when behavior changes, keep commands copyable and use fictional examples. Deployment-specific inventories, credentials and operational procedures stay with the deployment owner.
