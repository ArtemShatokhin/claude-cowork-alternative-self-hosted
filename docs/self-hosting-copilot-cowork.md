# Self-hosting Copilot Cowork: the open-source checklist

Microsoft Copilot Cowork cannot be self-hosted. Microsoft delivers it as a service inside your Microsoft 365 tenant, an administrator grants access through a spending policy, and usage is billed by consumption ([Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/copilot/cowork/cowork-admin-governance)). The tenant holds the data. Microsoft holds the software. That line decides more than a hosting checkbox. A team that needs the coworker's standing rules, skills and memory in a repository it can diff is choosing a different kind of product. Kortix, the open-source AI Operating System, is that different kind: the agents, their skills, the company memory and the connector configuration live as files in a git repo the company owns, and the whole stack runs on hardware you control.

## Where Microsoft says Copilot Cowork runs

Microsoft's product page is direct about the data. "Your data stays in your tenant and respects existing user and admin permissions" ([Copilot Cowork, Microsoft](https://www.microsoft.com/en-us/copilot/features/cowork)). The administrative guide says the same in operational terms: when Cowork runs a task, "it processes the user's files in a temporary, isolated environment inside the Microsoft 365 service boundary", and that boundary "is the security and data-processing boundary of your Microsoft 365 tenant" ([Manage Copilot Cowork, Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/copilot/cowork/cowork-admin-governance)).

Access is an entitlement inside that boundary, not an install. "To allow users to access Cowork, admins must enable usage-based billing," the same guide states, and a spending policy doubles as the access control: anyone in the scope of a policy that selects Cowork can use it. Cowork then appears in the Copilot app and works across Microsoft 365, Dynamics 365 and Fabric.

Two details matter. The customer does not supply the model: "Cowork ships with several models", and an admin can switch the Anthropic model family on or off while Auto keeps selecting from the rest. The one piece that runs on customer hardware is a browser tab, which the guide says "runs on the user's machine" and inherits the conditional access and DLP policies already applied to those users. A tab is not a deployment.

Microsoft describes an admin console, a service boundary, a billing policy and a model picker. It describes no package, image or installer a customer runs. Copilot Cowork has no self-hosted edition, and Microsoft documents none.

## What a tenant boundary covers, and what it leaves out

A tenant is a real boundary. It carries Purview policies, the unified audit log, conditional access and data residency, and those controls are often the reason to buy Microsoft. Microsoft states that "Copilot Cowork follows the same data residency model as Copilot."

The boundary wraps data, and four things still stay on Microsoft's side:

- **The application code.** There is no source to read, fork or rebuild, and no release you can pin to a version of your choosing.
- **The update path.** Capabilities arrive when Microsoft ships them, including preview features that may change or disappear.
- **The configuration store.** Custom skills are created in the product, where up to 50 are allowed, and Cowork discovers them "automatically at the start of each session". Microsoft documents no file layout, export or diff, so a skill change cannot be reviewed like code.
- **The model account.** Inference is billed through the tenant's usage-based plan, and the model families on offer are the ones an admin is allowed to enable.

Each follows from the shape of a managed service, and for a company already working inside Microsoft 365, that shape is the reason it fits. The tenant answer and the self-host answer reply to different questions.

## What Claude's hosting options change

Claude Cowork sits in a similar place, with one more option and the same ceiling. Anthropic merged Cowork and chat into one Claude, so the work Cowork did now starts from any conversation, rolling out to Pro and Max plans first ([Claude Cowork and chat are now one Claude](https://claude.com/resources/articles/cowork-is-now-claude)). The product page lists where it runs: "Use a Claude account or your own cloud provider: Amazon Bedrock, Google Cloud, or Microsoft Foundry" ([Claude Cowork](https://claude.com/product/cowork)).

The cloud-provider route is the closest thing either vendor offers to a customer-controlled deployment. It moves inference and billing into an account you already hold. The application, its updates and the store for skills, plugins and projects stay with Anthropic, and the models stay Claude models. A team that needs the software on its own server still has no install to run. [What Claude Cowork is](https://claudecoworkalternative.com/cowork) covers the product; hosting ends in the same place for both vendors.

## The four tests a self-hosted coworker has to pass

A security reviewer rarely asks "can we host it" and stops. The useful question has four parts, and each produces evidence a team can check.

| Criterion | Copilot Cowork | Claude Cowork | Kortix |
| --- | --- | --- | --- |
| Configuration | Custom skills created in the product; no documented repository | Skills, plugins and projects stored in the Claude account | Agents, skills and memory as files in one git repo |
| Model account | Microsoft bills usage; admins choose allowed model families | Claude models only, on Anthropic's or your cloud account | Any model provider with your own keys |
| Where sessions run | Temporary environment inside the Microsoft 365 service boundary | Anthropic's cloud, or your Bedrock, Google Cloud or Foundry account | Your server runs the stack; a sandbox provider runs sessions |
| Who approves work | The person approves actions; admins set spending limits | The person approves actions; admins set permissions | A person approves the change request; merge is default-deny |

The first column describes a service you buy. The third describes work you take on. The middle column sits between them, with more deployment choice than Microsoft offers and still no software you operate.

## How Kortix answers the four

The configuration test separates a repository from an account, so this page shows it rather than describes it. This is the header of the starter manifest every new Kortix project begins with ([the Kortix starter manifest](https://github.com/kortix-ai/suna/blob/dev/packages/starter/templates/base/kortix.yaml)):

```yaml
# Kortix project manifest. Source of truth for project-wide config that
# lives in git alongside the code. `kortix_version` pins the schema so
# the platform can evolve safely.
#
# Project layout — harness-neutral content at the root:
#   agents/<name>.md          one file per agent (`agents.<name>.file`)
#   skills/<name>/SKILL.md    skills, loaded by every harness
#   memory/                   the project brain (the `kortix-memory` skill)
#   tools/<name>.ts           project tools, loaded by every harness (`tools:` below)
kortix_version: 2
```

Those comment lines answer the first test without argument. A skill is a folder with a `SKILL.md`. Memory is plain files. An agent's behavior is a markdown file, and its governance is a block in the manifest. A change to how the company works arrives as a diff, and the diff can be reverted.

The other three tests follow from the same layout:

- **Model account.** Kortix takes any model provider with your own keys, set per agent, per session or per message. On a self-hosted instance the keys live in the instance's `.env`, so the model bill goes to your provider account.
- **Sessions.** The control plane is one Docker Compose stack on a Linux server you administer: frontend, API, LLM gateway and database. Sessions do not run on that box. Each one runs on the sandbox provider the install connects, with Daytona as the default and Platinum or E2B as alternatives, and each gets its own cloud computer and branch.
- **Approval.** Session work reaches `main` through a change request, and merge is default-deny for agents. A person reads the diff before it lands, and an agent can propose a change to its own configuration without being able to merge it.

Hosting is a choice you make at install. Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free. The [self-hosting guide](https://claudecoworkalternative.com/self-hosting) walks the install, and the repository's own [self-hosting runbook](self-hosting-kortix.md) carries the commands.

Kortix is open source. Read it, fork it, audit it. That is what makes the checklist checkable: the answers above are files and settings a reviewer can open before the first session runs.

## Applying the checklist to another candidate

The four tests work on products this page has not named. Ask for the commit where the agent's instructions live. Ask which account pays for inference and who can rotate that key. Ask where a session's files sit while it runs, and who can merge the result.

An answer of "inside our tenant" settles the question about data and none of the other three. That distinction survives a change of vendor, a change of model, and the next product that borrows the word cowork.

## FAQ

**Can Microsoft Copilot Cowork be self-hosted?**
No. Microsoft offers Copilot Cowork as a service inside the Microsoft 365 tenant, enabled by an administrator and billed by usage. Microsoft documents no installer, image or on-premises edition, and the task environment runs inside the Microsoft 365 service boundary.

**Is a Microsoft 365 tenant the same as self-hosting?**
No. The tenant is a data boundary you control through policy. The application, its updates and the model account stay on Microsoft's side. Self-hosting means running the software yourself, a different commitment from governing where data sits.

**Does Claude Cowork offer a self-hosted option?**
No. Claude Cowork runs on a Claude account or on your own Amazon Bedrock, Google Cloud or Microsoft Foundry account. The second route moves inference and billing into your cloud account, while the application and its configuration remain Anthropic's.

**What does a self-hosted coworker need at minimum?**
Configuration in a repository you can diff, model credentials in an account you control, sessions on infrastructure you choose, and an approval step before work lands. Kortix answers all four.
