# Data ownership and permissions on a self-hosted, open-source Kortix

Kortix is the open-source AI Operating System, so the data and the rules that govern it sit on your infrastructure.

## Where your data sits

A self-hosted Kortix instance stores everything in two directories under `~/.config/kortix/self-host/<instance>/` on your own box: `volumes/db/data` holds the Postgres database and `volumes/storage` holds file storage. The instance's `.env` file holds the secrets. That is the same disk you already back up and control, and it is the reason a security review can end with a yes.

Kortix keeps the configuration as files. A project is a git repo where agents are markdown, company memory is plain files, and `kortix.yaml` declares the machine image, the connectors and the triggers. You can `grep` the whole company, diff any change, and roll any part of it back.

Claude Cowork draws a different boundary. Anthropic runs Cowork in its own cloud or in your own Amazon Bedrock, Google Cloud, or Microsoft Foundry account, and your standing instructions, plugins, skills, and projects live in the Claude product, saved to your Claude account ([Claude Cowork, Anthropic](https://claude.com/product/cowork)).

## How Anthropic contains a Cowork session

Anthropic describes Cowork's isolation on its own engineering page. Cowork runs on the user's desktop and executes inside a full virtual machine on the platform's hypervisor, Apple's Virtualization framework on macOS and HCS on Windows. The VM has its own Linux kernel, filesystem, and process table. The user's selected workspace and `.claude` folder are mounted, and nothing else on the host is visible. Credentials stay in the host's keychain and never enter the guest machine ([How we contain Claude, Anthropic](https://www.anthropic.com/engineering/how-we-contain-claude)).

That is a real boundary for one person's desktop. It does not give a company a server it runs itself, and the configuration stays inside the Claude product.

## Permissions in Kortix

Kortix treats people and agents as principals and scopes what each one may reach.

- Per-resource permissions for people and agents. Roles, groups, and an audit trail.
- SAML 2.0 single sign-on and SCIM 2.0, so membership and roles come from your directory.
- Approval gates you set. They are off until you set them, so you choose where an agent asks first.
- Session work reaches `main` through a change request, and merge is default-deny for agents. A person reads the diff before it lands.
- An agent can edit its own configuration on its session branch and propose the change, and a person approves it.
- Connector policy is allow, ask, or block per tool call, down to the arguments the agent was given.
- Secrets are encrypted at rest with a key per project, and connector credentials are brokered server-side and never enter the machine.
- Every action is recorded. Reading, exporting and streaming the audit log depend on the plan.

On the Enterprise plan the same layer adds advanced RBAC, SAML SSO and SCIM directory sync with Okta, Microsoft Entra, and JumpCloud, audit logs, an SLA and DPA, and a choice of Cloud, VPC, or on-prem deployment ([Kortix pricing](https://kortix.com/pricing)).

## The permission layer in a security review

A self-hosted Kortix instance puts the data, the model keys, and the access rules in one place you own, and it leaves a record of what each agent did. For a company deciding between a closed desktop agent and a system it runs itself, that is the difference between trusting a vendor's promises and reading your own configuration. The [is Claude Cowork open source](https://claudecoworkalternative.com/is-claude-cowork-open-source) page covers the ownership question directly. [Try Kortix](https://claudecoworkalternative.com/) to read the permissions and the audit trail on a live project.
