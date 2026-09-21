# Delivery Workflow

## Control Plane

Use the control plane named in `docs/agents/issue-tracker.md`. For GitLab, the `gitlab-mcp` gateway owns transport selection and returns the operation receipt.

## Definition Of Ready

An implementation issue may receive the repository's ready-for-agent triage role only when it states:

- the source plan, spec, and ADR headings it implements;
- the bounded outcome and observable acceptance criteria;
- nearby non-goals;
- blocking issues or `None`;
- focused and full verification to run; and
- unresolved decisions, or `None`.

## Delivery Unit

One ready implementation issue normally delivers through one branch and one change request. The change request links the issue using the tracker’s closing syntax and records plan traceability, validation results, review outcome, and deviations.

An exception for a shared branch or combined change request requires a human decision recorded on every affected issue before implementation begins.

## Merge Authority

An agent may merge only when the issue remains ready for agent work, every acceptance criterion has evidence, required checks pass, focused and full validation are recorded, review has no unresolved findings, and every change-request discussion is resolved.

A failed required check, unavailable required validation, unresolved decision, or unresolved discussion moves the issue to the repository's human-intervention triage role. An agent does not approve its own exception.