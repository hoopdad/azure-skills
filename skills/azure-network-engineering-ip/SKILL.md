---
name: "azure-network-engineering-ip"
description: "Execute Azure network engineering workflows inspired by Mike's blog: PEP DNS validation, ASE planning, managed identity audits, and Terraform module decomposition."
---

# Azure Network Engineering IP

Use this skill for hands-on Azure network engineering tasks: troubleshooting private access, validating DNS, auditing managed identity, planning ASE networking, decomposing Terraform, or wiring Event Hub Kafka with managed identity.

## Engineering stance

Treat repeated manual investigation as a script or tool trying to escape. Prefer deterministic checks, reusable scripts, IaC validation, and small repeatable runbooks over long ad-hoc prompting.

## Core workflows

### 1. Private Endpoint DNS health check

Use when Private Endpoint, Private Link, portal tooling, CORS, browser PNA, or private access is failing.

Checklist:
- Identify resource hostname and expected privatelink zone.
- Resolve the public hostname from each relevant access path.
- Confirm CNAME chain reaches the privatelink hostname.
- Confirm final A record is private IP only.
- Confirm private DNS zone exists and is linked to every required VNet.
- Confirm on-prem or VPN clients can resolve through conditional forwarding or resolver path.
- Check for public IP leakage in the chain.
- Check browser Private Network Access/site-discovery permission if portal tools fail despite correct DNS.
- Produce remediation steps and a repeatable command/script.

Suggested PowerShell validation shape:
- Resolve-DnsName <resource-hostname>
- Resolve-DnsName <privatelink-hostname>
- Compare expected private IP to Private Endpoint NIC IP.

### 2. ASE v3 network planner/checker

Use when designing, deploying, or troubleshooting ASE v3.

Checklist:
- Subnet CIDR is sized intentionally, usually /24 for production planning.
- Subnet delegated to Microsoft.Web/hostingEnvironments.
- NSG allows required HTTPS and internal load balancer paths.
- Internal load balancing mode matches desired access model.
- DNS suffix and app hostnames resolve to internal load balancer IP.
- Zone-redundant and dedicated-host settings are not treated as interchangeable.
- Cluster settings and TLS posture are explicit.
- Provisioning time and deployment sequencing are acknowledged.
- Terraform stages network foundation before ASE before app.

### 3. Managed identity auditor

Use when validating passwordless access and least privilege.

Checklist:
- List system-assigned and user-assigned identities on the workload.
- Enumerate RBAC assignments at resource, resource group, subscription, and management group scopes.
- Flag Owner, Contributor, broad data-plane roles, stale identities, and shared identities with too much blast radius.
- Verify local authentication is disabled where the platform supports it and policy requires it.
- Verify runtime can acquire a token using DefaultAzureCredential or service-specific mechanism.
- Verify audit logs show identity-based access.
- Recommend least-privilege scopes and identity separation.

### 4. Event Hub Kafka managed identity wiring

Use when replacing Kafka connection strings or SAS keys with Azure managed identity.

Checklist:
- Create or reuse a user-assigned managed identity.
- Assign the proper Event Hubs data role at the narrowest workable scope.
- Disable local authentication when feasible.
- Configure application auth handler or SDK path for managed identity.
- Inject the identity into Container Apps, Functions, VM, AKS workload identity, or chosen runtime.
- Deploy and validate publish/consume through logs or telemetry.
- Document fallback risks if local auth remains enabled.

### 5. Terraform module decomposer

Use when reviewing or refactoring network/IaC modules.

Checklist:
- Separate hardened modules from deployment patterns.
- Keep hardened modules narrow, reusable, independently testable, and versioned.
- Move app-specific composition, role bindings, and glue into deployment patterns.
- Avoid monolith modules that bundle unrelated lifecycle concerns.
- Add README intent, examples, variables, outputs, and validation commands.
- Use remote state and locking for shared infrastructure.

## Output format

For troubleshooting:
- Finding
- Evidence
- Root cause hypothesis
- Deterministic validation command
- Remediation
- Follow-up hardening

For implementation:
- Steps
- Commands or IaC surfaces to inspect/change
- Validation gates
- Risks and assumptions

## Verify externally

Before making claims about current platform behavior, verify current Azure documentation, tenant policy, and live resource state when available.
