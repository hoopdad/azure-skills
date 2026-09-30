# Azure Network Engineering IP

**Skill folder:** `skills/azure-network-engineering-ip/`

Use this skill for **hands-on** Azure network engineering work: troubleshooting private access, validating DNS, auditing managed identity, planning ASE v3 networking, decomposing Terraform modules, or wiring Event Hub Kafka with managed identity.

## When to invoke

Ask for this skill (or let the agent auto-route to it) when you're:
- Troubleshooting a failing Private Endpoint / Private Link / portal tooling / CORS issue.
- Validating that a hostname resolves privately through the correct `privatelink` CNAME chain.
- Planning or checking an ASE v3 subnet, delegation, NSG, and internal load balancer configuration.
- Auditing managed identities and RBAC assignments for least privilege.
- Wiring Event Hub Kafka (or similar) to use managed identity instead of connection strings/SAS keys.
- Reviewing or refactoring Terraform network modules into hardened modules vs. deployment patterns.

## What it produces

See the SKILL.md `Output format` section — for troubleshooting: finding, evidence, root cause hypothesis, deterministic validation command, remediation, follow-up hardening. For implementation: steps, IaC surfaces to inspect/change, validation gates, risks/assumptions.

## Not for

High-level architecture reviews or target-state design decisions — use [`azure-network-design-ip`](../azure-network-design-ip/README.md) for that.

## Example

See [`examples/private-endpoint-dns-health-check.md`](../../examples/private-endpoint-dns-health-check.md) for a worked sample prompt and output.
