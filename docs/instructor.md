# Lab 1 instructor runbook

The [root guide](../README.md) is the complete mandatory learner path. This runbook supplies preparation, observable checkpoints, and answers, not an unpublished setup dependency.

## Preparation and boundaries

- Deliver this source on the instructor repository's default **dev** branch and provide the frozen starter SHA. Participants clone that branch, rename locally to **main**, and push only to their own empty approved repository.
- Verify the starter contains no active workflows and no nested Git repositories. All complete workflow templates remain in [the source mapping](source-alignment.md#source-to-checkpoint-map); the event snippet is an inactive fragment.
- Confirm Git/Terraform **1.16.3**/VS Code, ordinary Git push authentication, an approved empty public destination, Actions access, and eligible environment reviewer access. No GitHub CLI, Azure CLI, Node, external linter, or paid Copilot subscription is required for the participant path.
- Public repositories are intentional for supported required environment reviewers. For a policy-mandated private/internal repository, independently verify appropriate Enterprise feature support. Never make sensitive content public or remove an approval gate to work around licensing.
- Respect existing organization rules from the first import. Arrange a compliant bootstrap/PR path if direct main pushes are restricted. Do not teach bypasses, blanket write permissions, or a switch to `pull_request_target`.
- The **L1 cloud-free quality** check is intentionally branch-filtered and **must not be made required** in this demonstration. Preserve any pre-existing required checks. Manual dispatch alone does not establish an eligible required PR check.
- The `production` name is source-aligned but operates no Azure infrastructure. Configure required reviewers, Prevent self-review OFF, and Selected branches and tags -> Branch `main` only before activating the gate.
- Do not use a personal access token in a command, add a client secret, configure OIDC, or run Terraform plan/apply/destroy. Provider-free init and validate are sufficient. There is no provider lock file because there are no provider dependencies.
- The 75-minute allowance assumes readiness pre-work and access checks are already complete. Rehearse timing; do not conceal blocked gates to meet it.

## Teaching and observation matrix

| Guide checkpoint | Minutes | Observable success | Stop/recovery condition |
| --- | --- | --- | --- |
| 1 - Owned copy | 12 | Both remotes owned, empty destination, non-secret worksheet commit, default main, no run | Unexpected destination/history or protected import: reconcile without force-push |
| 2 - Install manual | 8 | Named manual workflow; local fmt/init/validate succeed; installation push has no run | Unexpected diff/version or an automatic event: compare installed source |
| 3 - Run manual | 6 | Exact `workflow_dispatch` / main / installed SHA; green format and validation | Missing default-branch workflow/permission/allowed action: diagnose the specific restriction |
| 4 - Trigger edit | 7 | Only `on` changed; fresh follow-up commit automatically produces a matching push run | A manual rerun or old trigger-edit SHA is not the fresh positive |
| 5 - PR | 7 | Same-repository main-target PR; synthetic merge SHA distinguished from head; current checks pass | Two runs are expected if both push and PR match; inspect event, not count alone |
| 6 - Filter pair | 7 | Nonmatching remote push without PR; same-workflow matching branch runs | No positive control means negative remains unproven |
| 7 - Failure/fix | 10 | Actual local and runner fmt exit 3; new repaired commit/run green | Other failing steps are not formatting evidence; do not merge while broken |
| 8 - Environment | 13 | Main guard passes, separate job waits, eligible review, same run completes | Immediate completion without review is a failed gate; correct settings and run fresh |
| 9 - Evidence | 5 | Actual URLs, commit identities, negative context, approval actor/time, retention decision | Mark unavailable observations BLOCKED/NOT TESTED, never PASS |

## Source and shell review cues

- [Manual template](../src/workflows/learn-actions-manual.yml): named workflow, job, and steps; `ubuntu-24.04`; ten-minute timeout; read-only contents permission; SHA-pinned checkout/setup; `persist-credentials: false`; `terraform_wrapper: false`.
- [Automatic reference](../src/workflows/learn-actions-automatic.yml): identical job behavior, with bounded events. Compare after the learner edits only `on`; never install a second quality copy just to get a green badge.
- [Event fragment](../src/snippets/l1-auto-events.yml): push matches source branches `main` or `feature/**`; PR matches **base** `main`. There is no path filter, tag event, schedule, or cloud action.
- [Gate template](../src/workflows/environment-gate.yml): manual-only; guard rejects other refs/events; dependent job additionally checks manual main and requests fixed `production`. No checkout, token write, environment secret, or deployment command is needed to demonstrate review.
- Linux shell values from event contexts/inputs enter `env`; fixed-format, quoted `printf` treats them as data. The optional note uses Bash `%q` so newlines/metacharacters are escaped in logs. Never add `eval` or direct expression interpolation of participant text into `run`.
- Runner identity output logs event SHA and actual checkout SHA; PR checkout normally uses GitHub's synthetic merge ref. The PR head is recorded separately. `github.actor` identifies initiation, not authoritative approval evidence.
- Local commands are PowerShell 5.1, not Bash. Terraform's full `-chdir=src/terraform` argument is quoted. No `&&`, native one-liner source rewriting, global Git configuration changes, or unreviewed bulk staging is needed.
- Intentional [broken fixture](../src/snippets/l1-format-fail.tf.txt) is valid HCL with only formatting defects. [The known-good fixture](../src/reference/main.tf.txt) is the deterministic recovery comparison. Never place reference/snippet directories under the active root or run recursive format against the whole repository.

## Optional Copilot review

The guide provides exact selected-block prompts; both forbid execution, pushing, and settings/cloud changes.
The explanation prompt selects only `on` and `permissions`. A correct answer says manual dispatch, read-only repository contents, and no push subscription.
The improvement prompt selects the replacement `on` block. Accept comments that clarify head-versus-base filtering, not a broadened trigger or permission change.
Copilot is optional. The source fragment and reference are the same deterministic fallback for every learner; do not grade on identical model wording.

## Knowledge-check answers

1. **Why did the initial push not run?** The published starter has no active workflow. After manual installation, that workflow listens only for `workflow_dispatch`, not `push`. These are distinct no-run observations.
2. **Which edit enables automation?** Replace the installed workflow's top-level `on` children using the supplied bounded event fragment. Actions policy must allow execution, but no Settings toggle converts manual events to automatic ones.
3. **What does the PR branch filter test?** `pull_request.branches` matches the target/base branch, `main`, not the feature/head branch. `push.branches` instead matches the pushed branch. A PR from `demo/**` to main qualifies even though its standalone push does not.
4. **Why can PR and local SHAs differ?** A `pull_request` run normally checks out the synthetic merge of head into base. Its `github.sha`/checkout SHA can differ from `github.event.pull_request.head.sha`. For the main-push and manual-main examples, compare the main event/checkout SHA to the recorded commit.
5. **How was the negative proven?** Verify the exact remote nonmatching SHA, committed event filters, Actions enablement, UI scope/time, and no associated PR; then get a successful fresh matching push using the same workflow content. Absence alone could also mean misconfiguration.
6. **Does `env` create an approval?** No. YAML `env` exposes process variables. A job's `environment: production` requests a GitHub deployment environment whose separately configured protection rules can hold the job. It is also distinct from a Terraform workspace.
7. **Who may approve deployment versus PR?** A configured required environment reviewer can approve; in this sandbox Prevent self-review is OFF, so that reviewer may also be the initiator. A PR author still cannot approve their own PR review. If repository rules require approval, a permitted peer must provide it; do not bypass. A solo repository without required PR approvals has no independent PR review, not self-review.
8. **What was deployed?** No cloud infrastructure or application. GitHub records a deployment-environment job and releases a message after approval. Formatting and static validation do not prove Azure credentials or authorization. Terraform has no provider/backend/resource here.

Additional prompts: a workflow contains jobs, jobs run on runners, and steps execute in order; setup/checkout are action steps while `run` steps are Bash. Least privilege means `contents: read` only, not OIDC or write access. `41 3 * * *` means daily 03:41 UTC in the pinned upstream example, with possible scheduling delays; this lab does not activate it.

## Rehearsal and honest reporting

Authoring diagnostics, local Terraform checks, and static workflow linting do **not** establish GitHub event, permissions, availability, or approval behavior. Complete a separately authorized owned-copy live rehearsal and retain the real evidence before claiming those pass.
Observe a pending gate before approval, not just a completed message. If the reviewer feature is unsupported or organization policy conflicts, leave a concrete BLOCKED status and route it to the owner.
Preserve failed formatting runs, fixes, source history, and public-safe evidence until the agreed retention deadline. No Azure cleanup is applicable. Any optional workflow disablement must not undermine existing required checks or other users' controls.

References: [manual dispatch](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manually-run-a-workflow), [event filters](https://docs.github.com/en/actions/how-tos/write-workflows/choose-when-workflows-run/trigger-a-workflow), [required checks](https://docs.github.com/en/pull-requests/how-tos/merge-and-close-pull-requests/troubleshooting-required-status-checks#checks-from-some-workflow-jobs-are-not-evaluated), [environment availability](https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments#required-reviewers), [deployment reviews](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/review-deployments), and [PR review restrictions](https://docs.github.com/en/pull-requests/how-tos/review-pull-requests/reviewing-proposed-changes-in-a-pull-request#submitting-your-review).
