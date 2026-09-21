---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement one ready issue or a bounded spec described by the user.

Read `docs/agents/delivery-workflow.md` before changing code. If it exists, it controls the Definition Of Ready, branch and change-request lifecycle, required evidence, and merge authority. If the issue does not meet that policy's Definition Of Ready, do not infer the missing decision: return it to the repository's human-intervention state with the blocker recorded.

Create the branch and change request the delivery workflow requires. Use /tdd where possible, at pre-agreed seams. Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Commit before running /code-review so its fixed-point diff contains the implementation. Record the review outcome, validation results, plan traceability, and deviations in the change request. Resolve every review discussion.

Merge only when the delivery workflow authorizes it. A failed required check, unavailable required validation, unresolved decision, or unresolved discussion requires the workflow's human-intervention path, never an agent-approved exception.
