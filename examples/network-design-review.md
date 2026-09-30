# Example: Target-state network design review

Exercises the `azure-network-design-ip` skill.

## Scenario

A team wants to move a public-facing web app and its SQL database behind Private Link, with access from a hub-spoke VNet, an on-prem VPN, and a CI/CD pipeline.

## Prompt

```
Use azure-network-design-ip to review a target-state design for:
- An App Service (public today) and Azure SQL Database
- Access required from: hub-spoke VNet, on-prem via VPN, GitHub Actions CI/CD, and developer workstations
- Requirement: no public data-plane access to SQL; App Service should only be reachable from the hub-spoke and VPN
- We use Terraform for all infrastructure

Produce a target-state architecture summary, access-path table, DNS design table,
identity/RBAC model, and validation checklist.
```

## Expected output shape

- **Target-state architecture summary** — Private Endpoints for App Service (inbound via VNet integration or App Service Environment) and SQL; hub-spoke topology with the app landing in a spoke VNet; VPN gateway in the hub for on-prem access.
- **Access-path table** — rows for hub-spoke traffic, on-prem/VPN traffic, CI/CD (e.g., self-hosted runner in-VNet or GitHub-hosted via private connectivity), and developer workstation access, each with the isolation mechanism and residual risk noted.
- **DNS design table** — `privatelink.azurewebsites.net` and `privatelink.database.windows.net` zones, which VNets they're linked to, and how on-prem DNS forwards to Azure Private DNS Resolver.
- **Identity/RBAC model** — user-assigned managed identity for the App Service to reach SQL (Azure AD auth only, local SQL auth disabled), least-privilege RBAC scoped to the resource group.
- **IaC module decomposition** — a hardened `private-endpoint` module and `private-dns-zone` module reused across App Service and SQL, versus a `webapp-sql-pattern` deployment pattern that composes them.
- **Validation checklist** — DNS resolution checks from each access path, NSG/route checks, no-public-IP checks, Terraform `validate`/`plan` gates, and a post-deploy smoke test hitting the app from the VPN path only.
- **Risks/assumptions to verify externally** — current Private Endpoint quota/IP consumption, multi-region DNS failover if the app expands to a second region, and tenant-specific hub-spoke/firewall standards.
