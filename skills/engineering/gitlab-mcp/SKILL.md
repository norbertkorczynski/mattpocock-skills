---
name: gitlab-mcp
description: "Route GitLab issues, branches, merge requests, discussions, pipelines, and project operations through a GitLab MCP-bound subagent. Use whenever work needs to read or change GitLab."
---

# GitLab MCP Gateway

All GitLab operations pass through the `gitlab-mcp-operator` custom subagent. The operator owns GitLab transport selection and returns an operation receipt to the parent agent.

The operator is the only agent that receives the GitLab MCP tool schema. It uses GitLab MCP first and selects `glab` when the MCP server does not expose the requested operation. This gateway keeps that schema out of the parent agent's context window.

## Dispatch

Delegate one complete GitLab request to `gitlab-mcp-operator`. The request must include:

- the intended read or mutation;
- the project path or identifier;
- known issue, branch, merge-request, discussion, or pipeline identifiers;
- exact content for a mutation;
- constraints that limit the operation; and
- the evidence required in the response.

The operator reads the necessary current state, performs the requested operation, and returns an operation receipt. The complete GitLab request belongs to the operator.

## Mutation Boundary

For a mutation, state the target resource and desired final state before delegation. The request bounds the operator to its stated GitLab changes.

If the request is incomplete, ask the user or retrieve the missing non-GitLab information before dispatching. If the `gitlab-mcp-operator` is unavailable, report the configuration blocker.

## Operation Receipt

The operator response must state:

- whether the requested operation completed;
- the project and resources read or changed;
- resulting identifiers and URLs when available;
- the final observable state; and
- any blocker or partial failure.