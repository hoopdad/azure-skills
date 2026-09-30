# Example: Private Endpoint DNS health check

Exercises the `azure-network-engineering-ip` skill (workflow 1: Private Endpoint DNS health check).

## Scenario

An App Service configured with a Private Endpoint is unreachable from an on-prem client over VPN, but works fine from inside the VNet.

## Prompt

```
Use azure-network-engineering-ip to troubleshoot why app.example.com
(App Service with a Private Endpoint) resolves and works from inside our VNet,
but times out from an on-prem client connected over VPN. Give me the
deterministic checks to run and likely root causes.
```

## Expected output shape

- **Finding** — on-prem client likely resolves the public hostname instead of the `privatelink` CNAME chain, landing on a public IP that's blocked by the app's access restrictions (or it resolves correctly but routing/NSG blocks the path).
- **Evidence** — compare DNS resolution from a VNet-joined host vs. the on-prem client.
- **Root cause hypothesis** — missing conditional forwarder from on-prem DNS to Azure Private DNS Resolver, or the private DNS zone isn't linked to the VNet the VPN gateway routes through.
- **Deterministic validation commands:**
  ```powershell
  Resolve-DnsName app.example.com
  Resolve-DnsName app.example.com.privatelink.azurewebsites.net
  # Compare the resolved private IP to the Private Endpoint NIC's IP in the Azure portal/CLI
  ```
- **Remediation** — add/fix the on-prem conditional forwarder to Azure Private DNS Resolver's inbound endpoint IP, confirm the private DNS zone is linked to the correct VNet, and re-test from the on-prem client.
- **Follow-up hardening** — add this resolution check as a repeatable script/runbook, and add a validation gate to the pipeline that fails if the hostname resolves to a public IP.
