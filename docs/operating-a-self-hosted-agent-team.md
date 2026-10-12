# Operating a self-hosted, open-source Kortix agent team

Kortix is the open-source AI Operating System, and a self-hosted instance runs the same system as Kortix Cloud. What changes is who owns the box. The way the agents work stays the same.

## The unit of work is a session

A session is one unit of agent work on its own cloud computer and its own branch. One isolated sandbox per session, and each session has its own isolated machine and branch. The agent can install, run and break anything it needs, because only what it commits survives. That isolation is what lets a team run many sessions at once without one agent reading another one's state.

## The three ways work runs

Kortix runs work on demand, human-assisted, automated.

- **On demand.** Ask in the web app, in Slack, in Microsoft Teams, or from the CLI, and the session starts now.
- **Human-assisted.** The agent works and checks in for the calls that matter, and an approval holds the turn until a person decides.
- **Automated.** A trigger starts a session with nobody present. Kortix has exactly two trigger types: cron and webhook, and a cron trigger fires on the clock while a signed webhook fires on an event.

A session can also start from a channel. Slack and Microsoft Teams are live, and email is experimental, per project.

## How work lands on main

Session work reaches `main` through a change request, and merge is default-deny for agents. Your team reads the diff and merges it or sends it back, so the repo is the reviewed record of what the company changed. An agent can edit its own configuration on its session branch and propose the change, and a person approves it. Nothing an agent produces becomes the live state of the company without a person's pass over the diff ([Kortix docs](https://kortix.com/docs)).

## Reach into your tools

Connectors reach 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP, so an agent can open tickets, update a CRM, query a warehouse, or post to a channel. Each tool call is set to allow, ask, or block, down to the arguments the agent was given, and connector credentials are brokered server-side and never enter the machine. The [permissions guide](data-ownership-and-permissions.md) covers the rules a reviewer will look for.

## Running the box as a team

The platform work stays small because the stack is one Docker Compose file. Every instance updates itself automatically, and a team that needs exact versions pins one with `kortix self-host update --tag <version>` or turns the updater off. Back up the two data directories and the `.env` file before a destructive command. Every action is recorded, and reading, exporting and streaming the audit log depend on the plan, so a team that answers to an auditor should check the plan before it promises a report.

## Scale and supervision

Kortix runs thousands of agents in parallel on one config, each on its own cloud computer, with no crossover between them. The work a person reviews stays proportional to the changes that matter, because the sessions that change nothing commit nothing.

## Where to go next

Moving an existing Claude Cowork setup onto this system is covered on the [migration page](https://claudecoworkalternative.com/migration). To start from nothing, [Try Kortix](https://claudecoworkalternative.com/) on a real task, or self-host the stack on your own hardware and run the first session.
