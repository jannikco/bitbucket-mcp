# Make a pull request easier to review

**Bitbucket + your AI assistant · powered by PipMCP**

You want to make a pull request easier to review, using the account your team has actually connected.

**Made for:** Teams reviewing a proposed app change

For real use, you will need PipMCP and access to Bitbucket.

## Ask your AI to set this up

Paste this into a coding assistant that can open project files and run commands, such as Codex, Claude Code or Grok Build. You do not need to understand the code first.

```text
Help me use this project: https://github.com/jannikco/bitbucket-mcp

I'm not a programmer. My goal is: Make a pull request easier to review.

Read the project instructions and set it up in a new folder without overwriting my work.
Guide me through PipMCP and its private key setup; never ask me to paste secrets into this
chat. Run a documented sample-data demo if available, then help me get a first small
result from my connected app. Explain the result in plain English. Ask before changing
records, sending messages or publishing anything.
```

## Three useful things to ask for

```text
List pull requests and inspect the one I choose.
```

```text
Prepare a pull request between the branches I specify. Show its title, branches and
description and wait before creating it.
```

```text
Write a pull-request handover from the title, branches and current state we reviewed.
```

## Your path to a real result

```mermaid
flowchart LR
  A["Ask your AI"] --> B["Connect PipMCP"] --> C["Make a pull request<br/>easier to review"] --> D["Review together"]
```

1. [Open PipMCP setup](https://pipmcp.com/github/start?app=bitbucket&integration=bitbucket-mcp&utm_source=github&utm_campaign=github-bitbucket-mcp&utm_content=people-first-readme) and sign in or create your account.
2. Connect the app in PipMCP with the permissions you want to grant.
3. Create the product API key through the official setup, add it privately to your AI project's environment, and ask your AI to try a small read.

**What to expect:** Connect Bitbucket in your PipMCP account, then use your PipMCP key for the supported actions. Review proposed changes before they are sent. Results depend on the connected account and its permissions; check the returned outcome before relying on it.

Add your PipMCP key privately in your project settings. Keep keys out of chat and public repositories.

## Keep going

- [Find another useful integration](https://github.com/jannikco/pipmcp-connectors)
- [Connect an API you control](https://github.com/jannikco/openapi-to-mcp)
- [Try the GitHub project](https://github.com/jannikco/github-mcp)

<details>
<summary>Technical reference: installation commands, exact capabilities and troubleshooting</summary>

<!-- people-first:technical:start -->
# Bitbucket MCP server

**Use Bitbucket from Claude Code, Codex, Grok Build and MCP clients through PipMCP. One PipMCP API key connects this package to private execution.**

[Activate with PipMCP](https://pipmcp.com/github/start?integration=bitbucket-mcp&app=bitbucket&utm_source=github&utm_campaign=github-bitbucket-mcp) · [Integration directory](https://jannikco.github.io/pipmcp-connectors/integrations/bitbucket/)

## Build something useful

Review branch changes and open a reviewed pull request. Read provider data first, then explicitly approve the selected change with operator write opt-in and a unique per-call approval key.

The package is an authenticated client: discovery, schemas, policy decisions and execution come from PipMCP. Provider implementation remains on the server.

## Quick start

Requires Node.js 22+. Clone this repository, then:

```sh
npm ci --ignore-scripts
npm test
npm start
# After configuring your provider in PipMCP and setting PIPMCP_API_KEY:
npm start -- --live
# Launch the stdio MCP bridge:
npm run server
```

Create a PipMCP API key with explicit connectors:read and connectors:call scopes. Set PIPMCP_API_KEY in the calling process environment. Provider credentials are configured in PipMCP. Multiple accounts require explicit PIPMCP_INTEGRATION_ID and PIPMCP_CONNECTION_ID from discovery. Existing keys do not gain scopes automatically.

## Built for coding agents

Install this repository's skill into your project:

```sh
node lib/cli.mjs agents install --client codex --scope project --project /path/to/project
# Use claude or grok for those clients.
```

Connect AI agents to Bitbucket using one PipMCP API key and actual hosted tool discovery. Native AGENTS.md, CLAUDE.md and project skills support the current coding session. No additional AI model key is required. [capabilities.json](capabilities.json) describes the package; [llms.txt](llms.txt) indexes its instructions.

## Inspect actual tools before calling

```js
import {PipMCPGatewayClient} from './lib/index.mjs';
const client = new PipMCPGatewayClient(); // PIPMCP_API_KEY
const inventory = await client.getTools("bitbucket");
console.log(inventory.data.tools);
// Select a real tool and validate your arguments against its returned schema.
// client.callTool(provider, actualToolName, reviewedArguments, {idempotencyKey});
```

PipMCP enforces workspace, role, connection, policy/approval and usage controls. Preserve the original idempotency key when retrieving/resuming a receipt. Pending and confirmation_required are not completed provider results; uncertain writes are held for reconciliation. The bridge never auto-approves a hosted action.

## Version 1 migration

This release uses the authenticated PipMCP gateway for real discovery/execution. Upgrade your MCP launch command and configure PIPMCP_API_KEY before reconnecting. Historical local runtimes remain available under their original MIT license in Git history. 

## Evidence and support

Transport, package and agent fixtures are tested. Authentication/provider access must be checked in your own configured workspace; synthetic fixtures do not establish live provider execution. Do not include credentials or customer records in public issues. MIT covers this public client.

## Provider setup reference

[Official provider documentation](https://developer.atlassian.com/cloud/bitbucket/rest/api-group-pullrequests/). Credentials are entered in PipMCP; this package does not accept or store provider secrets.

<!-- people-first:technical:end -->

</details>
