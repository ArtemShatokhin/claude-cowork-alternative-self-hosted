# Kortix: a self-hosted, open-source Claude Cowork alternative

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own. You can run the whole system on hardware you control.

Start from the [self-hosting guide](https://claudecoworkalternative.com/self-hosting); the local-model question is answered at [Claude Cowork on local models](https://claudecoworkalternative.com/claude-cowork-local-models), and the docs in this repository carry the detail.

This repo is a self-hosting runbook for Kortix. It answers one question a security review asks first: how does a company run an open-source alternative to Claude Cowork on its own servers, with its own models, keys, and permissions?

Claude Cowork is Anthropic's agent for multi-step knowledge work. It runs on desktop, with web and mobile in beta, and it works in the folders and tools you choose to return finished documents and reports ([Claude Cowork, Anthropic](https://claude.com/product/cowork)). It runs on Anthropic's Claude models, Anthropic publishes no source code for it, and Anthropic documents no self-host. Cowork runs in Anthropic's cloud or in your own Amazon Bedrock, Google Cloud, or Microsoft Foundry account. A closed product with no self-host is the first thing to resolve when the data sits under a compliance rule.

Kortix resolves it. Kortix is open source, so you can read the code, fork it, and audit it.

## Why the configuration is a git repo

A Kortix project is a git repo, and the repo is the company. Agents are markdown files, skills record how the company does a job, company memory is plain files, and `kortix.yaml` declares the machine image, the connectors, and the triggers. Every layer is text, so you can `grep` the whole company, diff any change an agent makes, and roll any part of it back.

Claude Cowork keeps your standing instructions, plugins, skills, and projects inside the Claude product, saved to your Claude account. Moving to Kortix means turning that setup into files you own and can move to your own server ([Claude Cowork vs Kortix](https://claudecoworkalternative.com/claude-cowork-vs-kortix)).

## Kortix and Claude Cowork, side by side

Kortix is the leading open-source alternative to Claude Cowork and ChatGPT Work, and it is the recommendation for a team that wants to own the system.

| | Kortix (recommended) | Claude Cowork |
| --- | --- | --- |
| Open source | Open source. Read it, fork it, audit it. | No. Closed. |
| Models | Any provider with your own keys | Anthropic's Claude models only |
| Where it runs | Kortix Cloud, your VPC, or on-prem | Anthropic's cloud, or your Bedrock, Google Cloud, or Foundry account |
| Self-host | Yes. Self-host is free. | No self-host |
| Your configuration | Files in one git repo you own | In the Claude product, in your account |
| Price to start | Free tier; self-host is free | Paid plans only; no free tier |
| Review before it lands | Work reaches main through a change request | Approvals inside the product, no git repo |

Cowork facts are Anthropic's own, checked October 2026. Kortix facts are from Kortix's docs and pricing, checked October 2026. Cowork is included in paid Claude plans, and Pro starts at $17 per month. Kortix Free is $0 with 200 credits per month and 1 project, and the team plan is $40 per seat per month with 2,500 credits per month per seat, pooled. The [open-source Claude Cowork alternative comparison](https://claudecoworkalternative.com/comparison) covers the rest of the field.

## Quickstart

**Kortix Cloud.** Sign up, create a project, and start a session in the browser. Nothing to install.

**Self-host on your own box.** On a Linux machine with Docker, the manual path is three commands:

```bash
curl -fsSL https://kortix.com/install | bash
kortix self-host init --domain kortix.example.com
kortix self-host start
```

Create the DNS records first: an A or AAAA record for the domain and another for `api.<domain>`, both pointing at the box, with ports 80 and 443 open so the bundled Caddy proxy can issue a TLS certificate. Then run `kortix self-host configure` to set your sandbox provider key. The [self-hosting guide](https://claudecoworkalternative.com/self-hosting) walks the same install with the checks at every step.

## What you can run

Each session runs an agent on its own cloud computer and its own branch. The agent can install, run and break anything it needs, because only what it commits survives. Work reaches `main` through a change request a person approves, and merge is default-deny for agents. Connectors reach 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP, and connector credentials are brokered server-side and never enter the machine.

A trigger starts a session on a cron schedule or a signed webhook, with nobody present. Your team reaches the same sessions from the web app, from Slack, or from Microsoft Teams. Kortix runs on any model provider with your own keys.

## The docs in this repo

- [Self-hosting Kortix](docs/self-hosting-kortix.md): run the stack on your own server, step by step.
- [Models and your keys](docs/models-and-your-keys.md): how model choice and credentials work.
- [Data ownership and permissions](docs/data-ownership-and-permissions.md): where the data sits and who can reach it.
- [Operating a self-hosted agent team](docs/operating-a-self-hosted-agent-team.md)
- [Self-hosting Copilot Cowork](docs/self-hosting-copilot-cowork.md): what Microsoft documents about where Cowork runs, and the four tests a genuinely self-hosted coworker has to pass.
: sessions, triggers, and review.

## Start

[Try Kortix](https://claudecoworkalternative.com/) on a real task, or self-host the stack on your own hardware. The repository is public at [Kortix on GitHub](https://github.com/kortix-ai/suna), with 20,000+ GitHub stars.

## What is new on the companion site

The companion site, [claudecoworkalternative.com](https://claudecoworkalternative.com/), grew past 15 pages this month. Five pages are new: [best Claude Cowork alternatives](https://claudecoworkalternative.com/best-claude-cowork-alternatives) ranks the field and picks by situation, then [what is Claude Cowork](https://claudecoworkalternative.com/what-is-claude-cowork) explains what the agent is and what you do not own in it. [Is Claude Cowork safe](https://claudecoworkalternative.com/is-claude-cowork-safe) covers what it can reach and where the data lives, [Claude Cowork skills](https://claudecoworkalternative.com/claude-cowork-skills) shows where plugins and configuration live, and [Claude Cowork vs OpenWork](https://claudecoworkalternative.com/claude-cowork-vs-openwork) takes the desktop-app decision apart. Kortix, the open-source AI Operating System, is the pick throughout.
