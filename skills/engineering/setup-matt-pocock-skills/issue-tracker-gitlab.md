# Issue tracker: GitLab

Issues and specs for this repo live as GitLab issues. All GitLab operations go through the `gitlab-mcp` gateway.

Call the Skill tool with `gitlab-mcp` for every GitLab read or mutation. The gateway delegates the complete operation to its operator subagent and returns its operation receipt.

## Conventions

- **Create, read, list, comment, label, and close issues**: delegate the desired final state and identifiers to the gateway.
- **Create, read, comment on, review, and merge merge requests**: delegate the desired final state and identifiers to the gateway.
- **GitLab calls comments "notes" and pull requests "merge requests".**

## Merge requests as a triage surface

**MRs as a request surface: no.** _(Set to `yes` if this repo treats external merge requests as feature requests; `/triage` reads this flag.)_

When set to `yes`, MRs run through the same labels and states as issues through the gateway:

- **Read an MR**: request its description, diff, and discussions.
- **List external MRs for triage**: request open MRs, then keep only MRs whose author is not a project member or owner.
- **Comment, label, or close**: request the exact final state.

Unlike GitHub, GitLab numbers issues and MRs separately, so `#42` is unambiguous once you know which surface the maintainer means.

## When a skill says "publish to the issue tracker"

Call the Skill tool with `gitlab-mcp`, then delegate creation of a GitLab issue.

## When a skill says "fetch the relevant ticket"

Call the Skill tool with `gitlab-mcp`, then delegate reading of the GitLab issue and its discussions.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a single issue with **child** issues as tickets.

- **Map**: a single issue labelled `wayfinder:map`, holding the Notes / Decisions-so-far / Fog body. Create it through the gateway. (On GitLab tiers with native epics, an epic may hold the map instead; a labelled issue works everywhere.)
- **Child ticket**: an issue carrying `Part of #<map>` at the top of its description and labels `wayfinder:<type>` (`research`/`prototype`/`grilling`/`task`). Once claimed, the ticket is assigned to the driving dev.
- **Blocking**: GitLab's **native blocking link**, the canonical, UI-visible representation. Ask the gateway to add the native link. Native blocking links are a Premium/Ultimate feature; on the free tier (or where unavailable) fall back to a `Blocked by: #<n>, #<n>` line at the top of the description. A ticket is unblocked when every blocker is closed.
- **Frontier query**: ask the gateway for the map's open children, then drop any with an open native blocker, an open issue in the `Blocked by` line, or an assignee; first in map order wins.
- **Claim**: ask the gateway to assign the issue to the current user, the session's first write.
- **Resolve**: ask the gateway to add the answer, close the issue, and append a context pointer (gist + link) to the map's Decisions-so-far.
