# Self-host Kortix, the open-source Claude Cowork alternative

Kortix is the open-source AI Operating System, and self-hosting runs the whole stack on hardware you control.

Self-hosting Kortix means running Kortix's own Docker Compose stack on your server instead of on Kortix Cloud. The company's data sits on disk you control, and the model keys and the infrastructure stay yours ([Kortix self-hosting docs](https://kortix.com/docs/host)).

Claude Cowork has no equivalent. Anthropic runs Cowork in its own cloud or in your own Amazon Bedrock, Google Cloud, or Microsoft Foundry account, and it documents no self-host ([Claude Cowork](https://claude.com/product/cowork)). Self-hosting is the part of the comparison a security review asks about first.

## What self-hosting installs

A self-hosted Kortix instance is one Docker Compose stack. It holds the frontend, the API, the LLM gateway, and the Supabase distribution. Agent sessions do not run on that stack. Each session runs on a separate sandbox provider you connect, and Platinum is the default provider, with Daytona available as a legacy fallback. The stack has no Redis and no separate worker, so there is no queue service to babysit.

## What the box needs

- A Linux machine with Docker.
- A domain with two DNS records: an A or AAAA record for the domain and another for `api.<domain>`, both pointing at the box's IP address.
- Ports 80 and 443 open, because the bundled Caddy proxy issues the TLS certificate on them.

## The first run, step by step

1. Install the `kortix` CLI.
   ```bash
   curl -fsSL https://kortix.com/install | bash
   ```
2. Point DNS at the box. Create the A or AAAA record for the domain and the record for `api.<domain>`, and open ports 80 and 443.
3. Initialize the instance.
   ```bash
   kortix self-host init --domain kortix.example.com
   ```
4. Start the stack.
   ```bash
   kortix self-host start
   ```
5. Watch it come up with `kortix self-host status`, `kortix self-host logs`, and `kortix self-host doctor`.
6. Set the sandbox provider key. `kortix self-host configure` is an interactive prompt for the sandbox provider key and, optionally, a managed-git token.

On a bare Linux box, a one-shot bootstrap script runs the install and the start in a single command. The manual path above performs the same steps and leaves each one visible.

## Evaluation mode with no domain

To try Kortix before you commit a domain, use a Cloudflare tunnel:

```bash
kortix self-host init --tunnel cloudflare
kortix self-host start
```

The tunnel URL changes on every restart, so treat evaluation mode as a trial and move to a domain before production.

## Models, secrets, and GitHub

The first run asks for no model key. Models are BYOK in the app: sign up in the dashboard, connect your key in the model picker, and the instance uses your own key by default. GitHub connects in the dashboard under Settings → Git. Every secret and signing key the instance uses lives in its `.env` file. `kortix.yaml` declares the machine image, the connectors and the triggers, and the secret names and grants, and secret values never appear in the repo. The [models and your keys](models-and-your-keys.md) guide covers provider choice and credential handling.

## Updates and backups

Every self-hosted instance updates itself automatically. Pin an exact version with `kortix self-host update --tag <version>`, or turn the updater off with `--auto-update off`. Kortix has no separate backup system. Each instance stores its data in two directories under `~/.config/kortix/self-host/<instance>/`: `volumes/db/data` for the Postgres database and `volumes/storage` for file storage. Back up both directories and the `.env` file before any destructive command. The API container carries a 640 MiB memory limit by default, and a larger host can raise it with `kortix self-host env set KORTIX_API_MEMORY_LIMIT=1024m`.

## Where to go next

The [self-hosting page](https://claudecoworkalternative.com/self-hosting) walks the install from a blank box, and the [install guide](https://claudecoworkalternative.com/install) covers the three-command cloud path for a team that does not want to run a server. When you would rather not run the box, Kortix Cloud runs the same code. [Try Kortix](https://claudecoworkalternative.com/).
