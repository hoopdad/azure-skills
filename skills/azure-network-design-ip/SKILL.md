---
name: "azure-network-design-ip"
description: "Apply Mike's blog-derived Azure network architecture IP to design reviews, secure PaaS, Private Link, ASE, identity, and IaC composability."
---

# Azure Network Design IP

Use this skill for Azure Network Design work: architecture reviews, target-state designs, private connectivity strategy, secure PaaS patterns, ASE decisions, identity-aware network design, and infrastructure-as-code composability.

## Blog-derived principles to apply

1. Private networking is only as reliable as DNS.
   - For Private Endpoints, verify the public hostname resolves through the privatelink CNAME path to a private IP.
   - Design for every access path: Azure VNet, hub-spoke, on-prem, VPN, developer workstation, portal tooling, and remote browser access.
   - Treat DNS forwarding, private DNS zone links, and resolver behavior as first-class architecture components.

2. Secure PaaS design is a sequence, not a single resource.
   - Network foundation comes before isolated PaaS.
   - ASE and similar isolated platforms need subnet sizing, delegation, NSG rules, internal load balancing, DNS namespace planning, and deployment staging.
   - Zone redundancy and dedicated hosts are different risk decisions. Zone redundancy addresses availability; dedicated hosts address isolation risk.

3. Managed identity is part of network security.
   - Prefer user-assigned managed identities and least-privilege RBAC over connection strings and secrets.
   - Design identity blast-radius boundaries per workload, tier, and environment.
   - For Event Hub Kafka and similar integrations, identity design must be part of the end-to-end architecture, not an app afterthought.

4. Hardened modules and deployment patterns are different artifacts.
   - Hardened modules should be atomic, reusable, tested, and narrow.
   - Deployment patterns compose modules for a workload scenario.
   - Do not hide application-specific glue inside a supposedly reusable network module.

5. Written decisions reduce future agent and engineer waste.
   - Produce ADRs, module READMEs, diagrams, and validation gates.
   - If a network rule must always hold, codify it with policy, IaC, test, or pipeline validation. Do not rely on prose alone.

## Design workflow

1. Identify the workload boundary.
   - Applications, environments, subscriptions, regions, data sensitivity, regulatory needs, users, and operators.

2. Identify access paths.
   - Inbound user traffic, admin/portal traffic, service-to-service traffic, data-plane traffic, CI/CD, monitoring, private tooling, and break-glass access.

3. Choose the isolation pattern.
   - Private Endpoint, service endpoint, ASE, Container Apps environment, private AKS, hub-spoke, or shared platform.
   - State what risk the pattern is solving and what risk it does not solve.

4. Design DNS explicitly.
   - Private DNS zones, VNet links, conditional forwarders, Azure Private DNS Resolver, on-prem forwarding, developer/VPN behavior, and multi-region implications.

5. Design identity and authorization.
   - Managed identities, RBAC scope, workload separation, local auth disablement where appropriate, and audit paths.

6. Decompose infrastructure.
   - Separate network foundation, DNS, private endpoints, identity, application platform, diagnostics, and deployment pattern layers.

7. Define validation gates.
   - DNS resolution checks, route checks, NSG checks, identity token acquisition, RBAC audit, no-public-IP checks, Terraform validate/plan, and post-deployment smoke tests.

## Decision prompts

Ask and answer:
- What must stay private, and from which access paths?
- Which names must resolve privately, and where?
- What happens when a user is not on the network?
- Is this isolation, availability, compliance, or operational convenience?
- What identity accesses the data plane?
- Which decisions belong in reusable modules versus workload patterns?
- What can be validated deterministically before deployment?

## Gaps to verify outside the blog

Always check current docs or tenant data for:
- Current Azure limits, service availability, and pricing.
- Multi-region Private DNS and failover design.
- Tenant hub-spoke, DNS resolver, firewall, DDoS, and policy standards.
- Private Endpoint quotas and IP consumption.
- Browser Private Network Access behavior and enterprise policy controls.
- Terraform backend/state standards.

## Deliverables

Prefer concise, practical outputs:
- Target-state architecture summary.
- Access-path table.
- DNS design table.
- Identity/RBAC model.
- IaC module decomposition.
- ADR-ready decision record.
- Validation checklist.
- Risks and assumptions requiring current docs or tenant confirmation.
