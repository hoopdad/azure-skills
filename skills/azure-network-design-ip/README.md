# Azure Network Design IP

**Skill folder:** `skills/azure-network-design-ip/`

Use this skill during **architecture-level** conversations: design reviews, target-state designs, private connectivity strategy, secure PaaS patterns, Azure App Service Environment (ASE) decisions, identity-aware network design, and infrastructure-as-code (IaC) composability.

## When to invoke

Ask for this skill (or let the agent auto-route to it) when you're:
- Reviewing or proposing a target-state Azure network architecture.
- Deciding between Private Endpoint, service endpoint, ASE, or a private compute platform (Container Apps, AKS).
- Designing DNS resolution across VNet, hub-spoke, on-prem, VPN, and browser-based access paths.
- Defining managed identity and RBAC boundaries for a workload.
- Deciding how to decompose Terraform/Bicep into hardened modules vs. deployment patterns.
- Producing an ADR, access-path table, or validation checklist for a design.

## What it produces

See the SKILL.md `Deliverables` section — typically a target-state summary, access-path table, DNS design table, identity/RBAC model, IaC module decomposition, ADR-ready decision record, and a validation checklist.

## Not for

Hands-on troubleshooting, live DNS/identity validation commands, or step-by-step runbooks — use [`azure-network-engineering-ip`](../azure-network-engineering-ip/README.md) for that.

## Example

See [`examples/network-design-review.md`](../../examples/network-design-review.md) for a worked sample prompt and output.
