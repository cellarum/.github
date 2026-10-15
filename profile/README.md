<div align="left">

# Cellarum — everything you have, connected

Cellarum (SEL-ər-əm), Latin for “of the storerooms” — a name rooted in keeping things organized, accounted for, and ready when needed.

Cellarum brings your devices, servers, networking, stock, purchases, and contracts into one shared record. Each asset has a place in the bigger picture: where it is, who it belongs to, what it connects to, and where it stands in its lifecycle. Departments maintain their own inventories, while authorized teams can follow relationships across the organization.

Floor plans, rack layouts, and dependency views connect physical assets to the applications and services they support. Inventories, stores, and warehouses run on the same engine, accessible through a web app, native apps, an API, and an MCP server.

**In early development.** The repositories are private until the first release.

</div>

## Product scope

Cellarum is being built as a complete infrastructure inventory and asset-management platform. The scope includes the operational workflows of an established asset system alongside detailed networking and server inventory. Features below are the product direction; early development does not mean every subsystem is available yet.

- **Devices and asset management**: computers, phones, printers, accessories, components and licence seats, with assignment, checkout and checkin, audits, maintenance, files and labels
- **Networking**: ONTs, wall jacks, patch panels, switches and Wi-Fi access points, including ceiling-mounted equipment. Named ports and their connections make a path traceable through the physical network
- **Servers and services**: physical servers, hypervisors, VMs and cloud instances, with compute and storage details, host/guest relationships, ownership, backup and monitoring context, and application dependencies
- **Physical inventory**: buildings, rooms, communications closets, racks and placements, connected through floor plans and rack elevations. Explicit asset locations determine placement, even when a device name reflects an old location; updated plans and incomplete mappings need clear review paths
- **Lifecycle and support**: purchases, warranties, leases, support and end-of-life dates, with firmware information for network equipment and renewal records for websites, certificates and contracts
- **Stock, procurement and stores**: counted consumables, supplier catalogs, requisitions, purchase orders and budgets, with departmental stores and loans on the same engine
- **Controlled visibility**: departmental inventories and role-based access, with sensitive topology protected across maps, relationships, search, exports and the API
- **Trustworthy records**: an event for every change, clear data provenance and freshness, and migration tools that reconcile missing assets and conflicting facts

Assets are received, assigned, moved, loaned, counted, renewed and retired through the same API. History records who changed what, when and why.

## Architecture

Cellarum is an API-first operational platform built around a shared model of assets, stock, locations, people, and relationships. Web, native apps, integrations, and agents use the same contract, permissions, and business rules.

The backend is modular: inventory, infrastructure, lending, stock, procurement, contracts, and stores build on the same foundation. These are connected parts of one platform, rather than separate inventories that need reconciling.

```mermaid
flowchart TB
    Clients["Web · native apps · agents"]
    Adapters["External integration adapters"]
    Identity["OpenID Connect provider"]

    subgraph Cellarum["Cellarum platform"]
        API["Versioned API contract<br/>Authentication · domain permissions · validation"]

        Model["Shared inventory model<br/>Classes · assets · stock · people · locations"]

        Relationships["Relationships and placement<br/>Connections · dependencies · hosts · racks · floor plans"]

        Workflows["Operational and commercial workflows<br/>Loans · audits · maintenance<br/>Purchasing · contracts · stores"]

        History["Transactional write layer<br/>Current state and immutable events"]

        Workers["Background workers — planned<br/>Renewals · notifications · webhooks"]
    end

    DB[("PostgreSQL<br/>Records · relationships · event history")]
    Files["File storage — planned<br/>Attachments · documents · floor plans"]
    Analytics["External analytics consumers — planned"]

    Clients --> API
    Adapters --> API
    Identity -. authentication .-> API
    API --> Model
    API --> Relationships
    API --> Workflows
    Model --> History
    Relationships --> History
    Workflows --> History
    History --> DB
    Workflows -. asynchronous work .-> Workers
    API -. authorized file access .-> Files
    API -. read-only datasets .-> Analytics
```

**A shared model, many views.** An asset's assignment, physical location, network connections, hosted applications, and commercial history belong to the same record. Tables, floor plans, rack elevations, and dependency maps expose different parts of that model. Relationships live in PostgreSQL alongside the assets they connect.

**History is part of every write.** Changes to current state and their immutable events commit in the same database transaction. The event log can rebuild the current-state tables, making history verifiable rather than an activity feed maintained separately.

**Permissions live in the API.** Department domains define ownership and access boundaries. Every client and integration passes through those checks; visibility must carry through searches, relationships, maps, and exports.

**Integrations stay outside the core.** External adapters translate telemetry, monitoring, supplier, finance, and ticketing data into the public API. Cellarum owns operational records and workflows; analytical consumers receive read-only datasets.

**One application architecture across deployment targets.** Docker Compose is the working self-hosted stack. Azure and AWS are planned Terraform-managed targets using the same application containers, with storage and background processing designed to work across all three.

The core API, permissions, inventory, stock, loans, relationships, event replay, and web app are implemented. Commercial workflows, background workers, general attachment storage, floor-plan editing, and complete native and agent clients remain in development or planning.

## Repositories

| | Repo | Stack | License | |
|---|---|---|---|---|
| Server | `cellarum-api` | Python · FastAPI | AGPL-3.0 | The API core and its OpenAPI contract |
| Server | `cellarum-app-web` | TypeScript · Next.js | AGPL-3.0 | Web app for staff and the stores |
| App | `cellarum-app-swift` | Swift · SwiftUI | MIT | Cellarum for Mac |
| App | `cellarum-app-csharp` | C# · WinUI | MIT | Cellarum for Windows |
| Tooling | `cellarum-mcp` | Python · FastMCP | AGPL-3.0 | Your inventory for AI agents |
| Deploy | `terraform-azurerm-cellarum` | HCL | MIT | Azure infrastructure module |
| Deploy | `terraform-aws-cellarum` | HCL | MIT | AWS infrastructure module |
| Deploy | `selfhosted-docker-cellarum` | Docker | MIT | Compose stack for one server |

## Open core

The **server** (API and web app) and the **MCP server** are **AGPL-3.0**; the **native apps**, **Terraform modules** and **deployment tooling** are **MIT**. A separate commercial license is available for organizations whose policies do not permit AGPL.
