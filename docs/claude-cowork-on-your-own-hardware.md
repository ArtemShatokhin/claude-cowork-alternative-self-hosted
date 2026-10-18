# Running Cowork-style agents on your own hardware

"Can it run on my own hardware?" is the question that most often decides between Claude Cowork and an open-source route, and the honest answer depends on which of three things a buyer means: *where the model inference happens*, *where the agent's sessions run*, and *who holds the configuration and the data*. Claude Cowork and Kortix answer those three differently, and the difference is a choice about ownership rather than a feature list.

## What Claude Cowork documents

Cowork is a feature of Claude, on web, desktop and mobile since September 16, 2026 ([release notes](https://support.claude.com/en/release-notes/claude-apps)). Anthropic documents two places it runs: a Claude account, or "your own cloud provider: Amazon Bedrock, Google Cloud, or Microsoft Foundry" ([product page](https://claude.com/product/cowork), checked October 2026). Both are Anthropic's software on infrastructure you may rent; neither is a version you host. Cowork's work continues in the cloud when the laptop closes, and scheduled tasks run with no device online — the sessions live in Anthropic's cloud, not on your machine.

There is no documented path to self-host Cowork, and no documented bring-your-own-model path: every route serves Claude models. If the requirement is an on-prem install, or a model on your own GPUs, Cowork is not the product that answers it.

## What a self-hosted system looks like

Kortix is the open-source AI Operating System, and self-hosting is a supported install rather than a hosted tier: the `kortix self-host` commands bring up the full stack on your own infrastructure. The same system runs three ways — Kortix Cloud, your VPC, or your own on-prem network — and self-host is free.

Three properties make that real rather than nominal:

- **The configuration is yours.** Agents, skills, shared memory and connector rules are files in one git repository the company owns. You can grep the whole company, diff any change, and roll any part of it back.
- **The model is a setting.** Any model provider with your own keys, or the ChatGPT plan you already pay for; a self-hosted or unlisted endpoint works too. Route the bulk of the work to a cheap model and reserve a frontier model for the steps that justify it.
- **The work is reviewed.** Every session runs on its own cloud computer, and session work reaches `main` through a change request a person approves. Merge is default-deny for agents.

## A practical order of operations

Start with one real job rather than a platform migration. Install Kortix (or run it in your VPC), connect the tools that job already uses, and point one agent at it. When the first change request lands, decide what the second job is. The install is a first run with six questions and no model key; models are brought in the app afterwards. This is the same sequence the [getting-started guide](https://kortix.com/blog/getting-started-with-kortix) walks through.

Two related reads: [which model to run and how to pick it](https://claudecoworkalternative.com/claude-cowork-local-models), and the [alternatives compared by shape](https://claudecoworkalternative.com/comparison).
