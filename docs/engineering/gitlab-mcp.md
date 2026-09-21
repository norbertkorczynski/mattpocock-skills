## What it does

`gitlab-mcp` routes every GitLab read and mutation through one operator [subagent](https://www.aihero.dev/ai-coding-dictionary/subagent). The parent agent packages the request and receives a compact operation receipt; only the operator sees the GitLab MCP tool catalog.

Transport selection stays inside the operator: it uses GitLab MCP first and `glab` when the MCP server lacks the required capability. That boundary is what reduces the parent [context window](https://www.aihero.dev/ai-coding-dictionary/context-window) without weakening the delivery workflow.

## When to reach for it

Type `/gitlab-mcp`, or the agent reaches for it whenever work must read or change GitLab: issues, branches, merge requests, discussions, pipelines, labels, or projects.

| Need | What happens |
| --- | --- |
| Read GitLab state | The operator fetches exactly the requested resources and returns their state. |
| Change GitLab state | The operator performs the explicit mutation and returns its receipt. |
| GitLab information is incomplete | The parent gathers non-GitLab details or asks the user before it dispatches. |
| The operator is unavailable | The request stops with a configuration blocker. |

## Prerequisites

The user profile must contain a hidden `gitlab-mcp-operator` custom agent with the installed GitLab MCP server and terminal access for its `glab` capability fallback. To remove GitLab tool definitions from the coding agent itself, select a custom parent agent that exposes ordinary coding tools and `gitlab-mcp-operator`, but not the GitLab MCP server.

## The gateway boundary

The useful unit is one complete GitLab request. The parent supplies the project, identifiers, desired outcome, exact mutation content, and expected evidence. The operator owns GitLab state discovery and execution, then returns the receipt.

That keeps authority and context cleanly separated. The parent can still decide what the repository needs, while the operator is the only component that chooses the GitLab transport.

## It's working if

- The main coding agent has no GitLab MCP tools in its tool list.
- Every GitLab mutation has an explicit target and desired final state before delegation.
- The GitLab response identifies the resources changed and their resulting state.
- The operation receipt names the transport used and the resulting GitLab state.

## Where it fits

`gitlab-mcp` is a delivery-workflow infrastructure skill. [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) records how a repository uses its tracker; [implement](https://aihero.dev/skills-implement), [to-tickets](https://aihero.dev/skills-to-tickets), and [code-review](https://aihero.dev/skills-code-review) use this gateway whenever that tracker is GitLab. [ask-matt](https://aihero.dev/skills-ask-matt) routes the rest of the skill set.