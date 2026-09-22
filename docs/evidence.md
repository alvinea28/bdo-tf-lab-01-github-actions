# Lab 1 evidence - fill from actual observations

**Status: NOT YET RUN.** These are collection fields, not successful-run claims.
Follow [the guide](../README.md); replace a field only after observing its result in your owned repository.
Use **PASS**, **FAIL**, **BLOCKED**, or **NOT TESTED** for each checkpoint. Never label a local check as a GitHub run.

## Ownership and local baseline

| Item | Observed value |
| --- | --- |
| Non-secret participant alias | Not recorded |
| Owned repository URL and approved public visibility | Not recorded |
| Instructor source URL, dev starter SHA, and confirmation | Not recorded |
| `origin` fetch URL | Not recorded |
| `origin` push URL | Not recorded |
| Empty destination checked before first push | Not recorded |
| Initial participant commit and default main confirmed | Not recorded |
| No active starter workflows; initial push caused no L1 run | Not recorded |
| Local Git version and Terraform 1.16.3 verified | Not recorded |
| Local fmt/check/init-without-backend/validate results | Not recorded |

## Event progression and failure recovery

Record full run/PR URLs and full SHAs. For each live run read the event/ref/SHA from **Report event, ref, and checked-out commit**, not just its green badge.

| Checkpoint | Required evidence | Observation / status |
| --- | --- | --- |
| 2 - Manual installation push | Installation SHA; committed dispatch-only event block; Actions enabled; time/filter scope of the no-run observation | NOT TESTED |
| 3 - Manual baseline | Run URL; `workflow_dispatch`; `refs/heads/main`; event/checkout SHA; green formatting and validation | NOT TESTED |
| 4 - Automatic main push | Fresh worksheet commit after trigger installation; run URL; `push`; `refs/heads/main`; matching SHA; no manual dispatch | NOT TESTED |
| 5 - PR | PR URL/base/head; run URL; `pull_request`; `refs/pull/<number>/merge`; event/merge SHA and separate PR-head SHA; current checks/review/merge outcome | NOT TESTED |
| 6 - Filter negative | `demo/l1-no-push` commit visible remotely; no PR opened; committed bounded event block; Actions policy state; UTC observation time and no L1 push run for that SHA | NOT TESTED |
| 6 - Matching positive | Fresh `feature/l1-filter-positive` SHA; same workflow content as negative; successful `push` run URL/ref; PR opened afterward | NOT TESTED |
| 7 - Real formatting failure | Local exit 3; broken commit; failed automatic run URL/event; exact **Check Terraform formatting** failure, not an unrelated error | NOT TESTED |
| 7 - Recovery | Local fmt/check/validate exit 0; new fix commit; new automatic run URL/event; green check; repaired PR merged under existing rules | NOT TESTED |

An absent run without a successful matching control does not establish a functioning branch filter.
A manual rerun of the old commit does not prove a new automatic event or a fix.
For PRs, a synthetic merge SHA can differ from the branch head; record both, not a misleading equality.

## Environment gate

| Item | Observed value / status |
| --- | --- |
| Exact environment name `production` | Not recorded |
| Required reviewer(s), using approved public identities | Not recorded |
| Prevent self-review unchecked/OFF | Not recorded |
| Selected deployment branches: Branch `main` only; no tag rule | Not recorded |
| Environment feature supported and organization rules respected | NOT TESTED |
| Gate-install PR URL; quality passed; no PR environment request | NOT TESTED |
| Manual gate run URL/event/ref/main SHA | NOT TESTED |
| Main guard succeeded; message job waiting before approval, UTC time | NOT TESTED |
| Pending-review screenshot or UI observation (sanitized) | Not recorded |
| Approval actor and time from deployment review UI | Not recorded |
| Same run completed after approval; message and summary observed | NOT TESTED |
| No secrets configured and no cloud operation executed | Not recorded |

If a gate ran without waiting, mark that run **FAIL** for the approval exercise. Configure the supported rule, then record a **fresh** pending/approved run separately; preserve the failed observation.
If protection is unavailable or forbidden by policy, record **BLOCKED** and the exact non-secret reason. Do not silently replace it with an unprotected job.

## Explain and retain

- My explanation of trigger versus check versus deployment approval: Not recorded.
- Why `env` does not create a deployment approval: Not recorded.
- Why the author may approve this sandbox deployment but not their own PR: Not recorded.
- Optional Copilot: selected block, accepted/rejected suggestion, or **not used**: Not recorded.
- Required organization checks/reviews remained intact: Not recorded.
- Retention deadline and optional demo-workflow disablement: Not recorded.
- Remaining blockers, owner, and next action: Not recorded.
- Final status: **NOT YET RUN**.

This cloud-free lab has no live app URL, Azure test result, resource ID, or Terraform state to collect. Never include tokens, credentials, raw state, private data, or screenshots exposing them.
