# Models and your keys on a self-hosted, open-source Kortix

Kortix is the open-source AI Operating System, and it is model-agnostic: any model provider with your own keys. A model provider is the service that serves the model an agent thinks with, and Kortix keeps that choice separate from the platform.

## What "your keys" means

Any model provider with your own keys. Or the ChatGPT plan you already pay for. Or sign in with your OpenCode Console account for OpenCode Zen and Go ([Kortix models docs](https://kortix.com/docs)). A self-hosted instance uses your own key by default, so the model bill goes to your provider account and not through Kortix.

Model choice is the cleanest split between the two products. Cowork runs on Anthropic's Claude models and cannot be pointed at another provider ([Claude Cowork, Anthropic](https://claude.com/product/cowork)). A team that wants to move to a cheaper model, a self-hosted open-weight model, or a second provider cannot do it inside Cowork, and a team that wants one vendor for models and platform together finds that split in [Claude Cowork vs Kortix](https://claudecoworkalternative.com/claude-cowork-vs-kortix).

## Pick a model per layer

Kortix is model-agnostic, so you pick the model per agent, per session, or per message, and swap it the day a better one lands. The harness that runs the agents is OpenCode, configured by files in the repo, so the model lives in that configuration and you can change it without touching the platform. You set the source per project, so a team can run a subscription sign-in on one agent and a provider key on another.

An OpenAI-compatible endpoint works too. Point Kortix at your own URL and run your own models behind it, and the agents reach them the same way.

## Where credentials live

- On Kortix Cloud, secrets are encrypted at rest with a key per project.
- On a self-hosted instance, the `.env` file holds every secret and signing key the instance uses.
- `kortix.yaml` declares the secret names and grants. Secret values never appear in the repo.
- Connector credentials are brokered server-side and never enter the machine, so an app password for Gmail or Slack is not readable from the session that uses it.

For a secret that a session does receive, environment is the default exposure. Egress-enforced is experimental: the sandbox holds a handle, Kortix substitutes the real value outside the sandbox, only on approved HTTPS hosts, and an echoed value comes back as `[REDACTED]`. A computed credential, such as an HMAC signing key or an SSH key, must stay on the environment exposure, because the code has to hold the value to compute with it. The [data ownership and permissions](data-ownership-and-permissions.md) guide covers who can use a secret and how approvals gate an agent's calls.

## Where to start

[Try Kortix](https://claudecoworkalternative.com/) and connect a key in the model picker, or read the [is Claude Cowork open source](https://claudecoworkalternative.com/is-claude-cowork-open-source) page for the ownership question behind the model choice.
