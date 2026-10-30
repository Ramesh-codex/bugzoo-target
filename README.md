# bugzoo-target

A small order-processing library (`library/`) with a pytest suite (`tests/`).
It is the live target for [agent-bugfixer](https://github.com/Ramesh-codex/agent-bugfixer):
a Researcher → Coder → Reviewer agent team that proposes fixes for issues
here. A human must approve every fix before anything is pushed.

## How to use the agent

Only the repository owner can trigger it. Labels and commands from anyone
else are ignored.

1. **Open an issue** that describes the bug, then add the label **`agent-fix`**.
   The `agent-propose` workflow runs the agents on a copy of this repo. It then
   posts one comment with:
   - the root cause,
   - the proposed diff,
   - the Reviewer's reasons,
   - the test result (full suite in a sandbox with no network),
   - the estimated cost,
   - or, if the Reviewer escalated, the question it needs you to answer.

   The issue is labeled **`needs-approval`** (the Reviewer approved and the
   tests pass) or **`needs-decision`** (any other outcome). Nothing is pushed.
2. **Decide**, with a comment that contains only the command:
   - **`/approve`**: the `agent-approve` workflow takes the exact patch from
     that run. It checks that the patch changes nothing under `tests/` (or in
     test config or CI), pushes it to branch `agent/issue-<N>`, and opens a
     **draft** PR that says `Refs #<N>`.
   - **`/reject`**: labels the issue **`rejected`**. Nothing else happens.

The tests are the specification. The agents may not change them, and a patch
that touches them is refused.

## Running the tests

```bash
python -m pytest
```
