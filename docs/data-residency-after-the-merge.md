# Claude Cowork data residency: what an open-source self-host changes

A residency review starts with where the data sits. It rarely ends there. The reviewer also asks who controls the application code, the data store, the model keys, the agent configuration, and the audit trail. Anthropic answers the first for Claude Cowork and only part of the rest. Kortix is the open-source AI Operating System. It answers all five controls, with the control plane and the configuration on hardware you run, and sessions on the sandbox provider you connect.

The [what is Claude Cowork](https://claudecoworkalternative.com/what-is-claude-cowork) page covers the product; [is Claude Cowork safe](https://claudecoworkalternative.com/is-claude-cowork-safe) covers its reach.

## The five controls, and why location is the easy one

A region name can satisfy the first question and leave the review open. If the vendor holds the code, the accounts and the configuration, a copy of your files in Frankfurt does not let you leave, audit a session, or prove which agent version ran.

### What each control decides

- Application code: can you read it, fork it, run the build?
- Data store: which disk holds sessions, files and memory?
- Model keys: whose account pays, and who can rotate the key?
- Agent configuration: where do instructions and grants live?
- Audit trail: who records actions, for how long, and can you export them?

A vendor that answers "your region" and "not published" four times has answered one fifth.

## Where Claude Cowork runs after the merge

Cowork and chat became one product on September 16, 2026. Anthropic's help article says the distinction is removed and that "these are merged into a single conversation", so file work and scheduled runs are available from any conversation ([Claude Cowork and chat are one Claude](https://support.claude.com/en/articles/16761823-claude-cowork-and-chat-are-one-claude)); the [release notes](https://support.claude.com/en/articles/12138966-release-notes) record the date.

### Cloud sessions are the default

Anthropic's admin article states: "Cowork sessions run in the cloud by default: the agent loop and code execution run on Anthropic's servers, and sessions and files are saved to the member's Claude account" ([architecture overview](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview)). Local execution remains for older desktop deployments; a cloud session reaches a local file only through Claude Desktop, while the app is online.

### No documented self-host

Anthropic documents no self-host. The product page offers "a Claude account or your own cloud provider: Amazon Bedrock, Google Cloud, or Microsoft Foundry" ([Claude Cowork](https://claude.com/product/cowork)). Procurement asks about the second route next.

## A cloud marketplace route is a deployment mode, not a self-host

### What the route changes

Anthropic's third-party guide describes what that route changes. The app runs from a bundled local web application, conversation history stays on the device, and inference goes to the endpoint you configure ([Claude Desktop on third-party](https://claude.com/docs/third-party/claude-desktop/overview)). Configuration is delivered through MDM, a bootstrap server, or the admin console, and a person cannot edit what MDM delivers. On Google Cloud and Bedrock, inference runs in your chosen region, with history on local disk.

Prompts go only to your endpoint, history does not land in a Claude account, and the region is yours. For a narrow requirement, that can be enough.

### What stays with Anthropic

It is still not a system you run. The application is Anthropic's bundled build, with no published source to read or patch. No team service sits on your infrastructure: the app runs on each device, and on an SSH host for remote sessions. Configuration is a policy pushed to machines, not a file your team reviews. Telemetry is an OpenTelemetry export, metadata by default.

The route moves inference and conversation storage. It leaves the application, its update path and its configuration with Anthropic. A review that needs a service you operate does not pass on a cloud choice.

## What self-hosting requires, using this repository as the example

Self-hosting puts the control plane, the data store and the configuration on your side, with sessions on the sandbox provider you connect. Kortix ships it as an install: "Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free."

### The install

The stack is a Docker Compose install on a Linux server you own. The [self-hosting guide](self-hosting-kortix.md) gives the path:

```bash
curl -fsSL https://kortix.com/install | bash
kortix self-host init --domain kortix.example.com
kortix self-host start
```

The guide separates the stack from sessions. The frontend, API, LLM gateway and database run on your server. Sessions do not: "Each session runs on a separate sandbox provider you connect, and the default provider is Daytona, with Platinum and E2B also supported." Self-hosting the control plane is not running every session on the same box.

### Where the data sits

The [data ownership guide](data-ownership-and-permissions.md) puts Postgres in `volumes/db/data` and file storage in `volumes/storage`, under `~/.config/kortix/self-host/<instance>/`, with secrets in the instance's `.env`.

### The manifest and the session boundary

Configuration is a file in the repo. The same guide says `kortix.yaml` "declares the machine image, the connectors and the triggers, and the secret names and grants", and that secret values never appear in the repo. A v2 manifest is short ([kortix.yaml reference](https://kortix.com/docs/project/manifest)):

```yaml
kortix_version: 2
default_agent: kortix
env:
  optional: [STRIPE_API_KEY]
agents:
  kortix:
    file: agents/kortix.md
    connectors: all
    secrets: all
    skills: all
    kortix_permissions: all
```

`connectors` and `secrets` name what the agent may reach; `kortix_permissions` names what it may do. A reviewer can diff a grant before it merges.

Sessions add the isolation boundary: "One isolated sandbox per session. Each session has its own isolated machine and branch." Work reaches `main` through a change request. Merge is default-deny for an agent unless its grant includes `project.gitops.merge`. The starter manifest above uses `kortix_permissions: all`, so replace it with a list that omits that grant when a person must merge every change. Connector credentials stay outside the session: "Connector credentials are brokered server-side and never enter the machine."

The [self-hosting walkthrough](https://claudecoworkalternative.com/self-hosting) walks the same commands.

## The control table

| Control | Kortix self-hosted | Cowork on Anthropic's cloud | Cowork via a cloud marketplace route |
| --- | --- | --- | --- |
| Application code | Open source. Read it, fork it, audit it. | Anthropic's; source not published | Anthropic's desktop build; source not published |
| Data store | Postgres and file storage on your disk | Sessions and files in the member's Claude account | Conversation history on the device |
| Model keys | Any model provider, your own keys | Claude account | Your provider credentials, set by administrators |
| Agent configuration | Agents and skills as files in your git repo | Instructions, skills, plugins in the Claude product | Managed policy set by administrators |
| Audit trail | Every action recorded; export depends on plan | Compliance API and OpenTelemetry export | OpenTelemetry export; metadata by default |
| Open source | Yes | No | No |

Sources: Kortix cells from [Kortix docs](https://kortix.com/docs), the [data ownership guide](data-ownership-and-permissions.md) and the [self-hosting guide](self-hosting-kortix.md). Anthropic cloud cells from the [architecture overview](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview) and [product page](https://claude.com/product/cowork). Marketplace cells from [Claude Desktop on third-party](https://claude.com/docs/third-party/claude-desktop/overview). "Not published" means Anthropic states nothing for that control.

## Questions buyers ask

### Does Claude Cowork run locally?

It can, in two cases. Local execution remains for older desktop deployments, and the third-party deployment runs the app and stores conversation history on the device. A standard Cowork session runs in the cloud by default ([architecture overview](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview)).

### Is claude.ai self-hosted?

No. Anthropic operates claude.ai and documents no install on customer infrastructure ([Claude Cowork](https://claude.com/product/cowork)). The nearest route, Claude Desktop on third-party, is a desktop app that sends inference to your provider. It is not a claude.ai instance you run.

### Is there a difference between Claude and Claude Cowork?

Less than there was. Since September 16, 2026, Anthropic has merged them into one conversation: Cowork's task work is available from any conversation, and tasks, projects, connectors, skills and files carry over. Team organizations, and Enterprise organizations that have not enabled the unified experience, still see separate options ([architecture overview](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview)).

### How does Claude Cowork operate?

You give it an outcome rather than a sequence of steps. The agent loop and code execution run in an isolated sandbox, created for the session and removed when it ends. It reads connected folders, uses connectors, opens a browser, and returns finished files. A permission setting decides whether it asks before each action ([architecture overview](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview)).

## What to put in front of the reviewer

Ask for five answers, one per control, and a source for each. A vendor that cannot name the code, the store, the keys, the configuration and the log has not passed, whatever region it names. If you need a service you operate, Kortix is the open-source alternative built for that.

Start with [Try Kortix](https://kortix.com), read [Kortix docs](https://kortix.com/docs), or audit the code at [Kortix on GitHub](https://github.com/kortix-ai/suna).
