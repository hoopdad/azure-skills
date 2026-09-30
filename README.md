# Azure Skills Plugin

A skills plugin for [GitHub Copilot CLI](https://docs.github.com/copilot/how-tos/use-copilot-agents/use-copilot-cli) and [Claude Code](https://code.claude.com/docs/en/agent-sdk/plugins) that packages Mike's blog-derived Azure network architecture and engineering IP into two complementary skills:

| Skill | Purpose |
|---|---|
| [`azure-network-design-ip`](skills/azure-network-design-ip/SKILL.md) | Architecture-level: design reviews, secure PaaS, Private Link, ASE, identity-aware design, IaC composability. |
| [`azure-network-engineering-ip`](skills/azure-network-engineering-ip/SKILL.md) | Hands-on: PEP/DNS validation, ASE v3 planning, managed identity audits, Event Hub Kafka identity wiring, Terraform module decomposition. |

## Directory layout

```
azure-skills/
├── plugin.json                 # GitHub Copilot CLI plugin manifest
├── .claude-plugin/
│   └── plugin.json             # Claude Code plugin manifest
├── skills/
│   ├── azure-network-design-ip/
│   │   ├── SKILL.md
│   │   └── README.md
│   └── azure-network-engineering-ip/
│       ├── SKILL.md
│       └── README.md
├── examples/
│   ├── README.md
│   ├── network-design-review.md
│   └── private-endpoint-dns-health-check.md
└── README.md
```

This layout follows both ecosystems' conventions at once:
- **GitHub Copilot CLI** discovers plugins via a root `plugin.json` and loads skills from `skills/<name>/SKILL.md`.
- **Claude Code** discovers plugins via `.claude-plugin/plugin.json` and loads the same `skills/<name>/SKILL.md` files.

No duplication of skill content is required — both hosts read from the same `skills/` directory.

## Using this plugin

### GitHub Copilot CLI

Load the plugin directory directly:

```powershell
copilot --plugin-dir C:\Users\mikeolivieri\source\azure-skills
```

Or install/manage it interactively from within a session:

```
/plugin
```

Once loaded, invoke a skill by name or let Copilot route to it automatically when your prompt matches, e.g.:

```
Use azure-network-design-ip to review this hub-spoke design for Private Link and DNS gaps.
```

### Claude Code

Point Claude Code at this directory as a local plugin (see `/plugin` or your `settings.json` `plugins` config), then reference a skill by name:

```
Use azure-network-engineering-ip to validate Private Endpoint DNS for my App Service.
```

## Skills vs. examples

- `skills/*/SKILL.md` — the authoritative skill definitions (frontmatter `name` + `description`, followed by the workflow instructions the agent follows).
- `skills/*/README.md` — human-oriented explanation of when/why to use each skill.
- `examples/` — worked, end-to-end sample prompts and expected outputs showing each skill in action.

## Contributing

Keep skills narrow and deterministic:
- Design IP → `azure-network-design-ip` (architecture, decisions, ADR-style output).
- Engineering IP → `azure-network-engineering-ip` (runbooks, checklists, validation commands).
- Add a matching example under `examples/` for any new workflow you add to a skill.
