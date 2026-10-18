# The open-source Claude Cowork alternatives, compared by shape

Kortix is the open-source AI Operating System, and this guide sorts the open-source field that answers the same question Claude Cowork asks: who runs the agent, who picks the model, and who holds the work. Since September 16, 2026, Cowork itself is a feature of Claude, available from any conversation on web, desktop and mobile (Anthropic's [release notes](https://support.claude.com/en/release-notes/claude-apps)). What did not change: it is Anthropic's software, it runs Claude models, and there is no self-hosted version ([Claude Cowork product page](https://claude.com/product/cowork), checked October 2026).

The alternatives divide by shape, and shape decides the fit. A desktop agent serves one person on one machine. A system a team owns serves a company. Both are legitimate answers; they answer different asks.

## The desktop agents

[OpenWork](https://github.com/different-ai/openwork) is a desktop app for macOS, Windows and Linux built on the OpenCode harness. Its own repository describes an open-source alternative to Cowork with models from more than 50 providers on keys you supply, plus a local model through Ollama. A control plane called OpenWork Den adds shared skills and policies for small teams. The desk is the product.

[Kuse Cowork](https://www.kuse.ai/open-cowork/features) is a lightweight desktop agent written in Rust. Its feature page leads with local-first execution, bring-your-own-key connections to Anthropic, OpenAI and Gemini, and local models through Ollama.

[Eigent](https://www.eigent.ai) calls itself an open-source cowork desktop that runs several agents as one coordinated workforce, on your machine or on infrastructure your team controls, with your own keys or local models. Its hosted plan is $19.99 a month after a trial (checked October 2026).

## The team workspaces

[Agenta](https://agenta.ai) describes an open-source workspace where a team builds persistent agents, each with its own instructions, files, skills, tools, permissions and schedule, and reviews come back through Slack or Telegram. The shape is closer to what a team needs than any desktop app.

[Kortix](https://kortix.com) is the team system in this field. Agents, skills, memory and connector configuration are files in one git repository the company owns, every session runs on its own cloud computer, and work lands through a change request a person approves. Open source: read it, fork it, audit it ([Kortix on GitHub](https://github.com/kortix-ai/suna)). Model choice is a setting per agent, per session or per message, on your own keys. Run it on Kortix Cloud, in your VPC, or on your own on-prem network; self-host is free.

## How to choose

If the ask is one person's assistant on one machine, the desktop agents fit, and Cowork itself remains a strong proprietary option on Anthropic's terms. If the ask is a company's agent workforce that survives hardware changes, onboarding and audits, the deciding questions are: where does the configuration live, can the team diff it, and who reviews the work. A repository you own answers all three; a desktop app answers none of them at company scale.

Related: the [self-hosting guide](https://claudecoworkalternative.com/self-hosting) walks the install, and the [full comparison](https://claudecoworkalternative.com/comparison) puts the field side by side with Kortix first.
