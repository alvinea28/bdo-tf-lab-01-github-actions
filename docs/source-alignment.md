# Lab 1 source alignment and attribution

## Frozen upstream

This workshop is informed by Microsoft's MIT-licensed [Azure-Samples/terraform-github-actions at commit 2e6dee79491254d4eea193b1691d2c85fd32af0f](https://github.com/Azure-Samples/terraform-github-actions/tree/2e6dee79491254d4eea193b1691d2c85fd32af0f), not its moving default branch.
Preserve [the workshop MIT license](../LICENSE), including **Microsoft Corporation** and **2026 Alvine Aurelio** attribution. The original [upstream license](https://github.com/Azure-Samples/terraform-github-actions/blob/2e6dee79491254d4eea193b1691d2c85fd32af0f/LICENSE) governs the upstream material; workshop modifications are supplied under the same MIT terms.

| Pinned upstream source | Observed source concept | Deliberate Lab 1 adaptation |
| --- | --- | --- |
| [Terraform Unit Tests workflow](https://github.com/Azure-Samples/terraform-github-actions/blob/2e6dee79491254d4eea193b1691d2c85fd32af0f/.github/workflows/tf-unit-tests.yml) | Push-triggered init without backend, validation, formatting, scanner/reporting | Teach manual first, then bounded push/PR events; no provider/scanner/report upload or permissions beyond read-only contents. These are quality checks, not native Terraform unit tests. |
| [Plan/apply workflow](https://github.com/Azure-Samples/terraform-github-actions/blob/2e6dee79491254d4eea193b1691d2c85fd32af0f/.github/workflows/tf-plan-apply.yml) | Main/PR responsibilities and `production` environment | Preserve the environment name only; separate manual main-only message job with approval. No plan/apply, saved artifacts, Azure login, or cloud identity. |
| [Drift workflow](https://github.com/Azure-Samples/terraform-github-actions/blob/2e6dee79491254d4eea193b1691d2c85fd32af0f/.github/workflows/tf-drift.yml) | Manual event and `41 3 * * *` scheduled example | Read the daily 03:41 UTC expression; do not activate a schedule or drift workflow. |
| [Root Terraform](https://github.com/Azure-Samples/terraform-github-actions/blob/2e6dee79491254d4eea193b1691d2c85fd32af0f/main.tf) | Azure resource group and storage backend | Replace with a purpose-built variable/local/output fixture under [src/terraform/main.tf](../src/terraform/main.tf). No provider, resource, or backend. |
| [Upstream guide](https://github.com/Azure-Samples/terraform-github-actions/blob/2e6dee79491254d4eea193b1691d2c85fd32af0f/README.md) | GitHub Actions and separately configured cloud prerequisites | Independent sanitized owned-copy exercise; no cloud prerequisite. Git copies do not inherit repository settings or environments. |

The workshop's manual-first event sequence, negative filter control, intentionally broken format fixture, exact pins, inactive packaging, bounded Copilot prompts, and main-only environment guard are **workshop adaptations**, not assertions that upstream already supplies them.

## Frozen toolset

| Component | Pin | Use |
| --- | --- | --- |
| Terraform | **1.16.3** | [.terraform-version](../.terraform-version), [exact required version](../src/terraform/versions.tf), local instructions, and runner setup |
| Checkout | [3d3c42e5aac5ba805825da76410c181273ba90b1](https://github.com/actions/checkout/commit/3d3c42e5aac5ba805825da76410c181273ba90b1) (**v7.0.1**) | SHA pinned; `persist-credentials: false` |
| Setup Terraform | [dfe3c3f87815947d99a8997f908cb6525fc44e9e](https://github.com/hashicorp/setup-terraform/commit/dfe3c3f87815947d99a8997f908cb6525fc44e9e) (**v4.0.1**) | SHA pinned; `terraform_wrapper: false` |
| Runner | **ubuntu-24.04** | Explicit hosted runner image label, not a promise of an immutable VM image |

These pins were supplied as verified inputs to this authoring task. The [authoring report](daily%20work%20report/2026-09-22.md) separately records checks actually executed; pin selection or local linting is not a claim of a successful hosted run.
No provider/module dependency, lock file, Node package, or extra learner tool is required. Terraform commands consistently target the sole active root; shell working-directory defaults do not redirect action inputs.

## Source-to-checkpoint map

| Guide checkpoint | Exact supplied source / copy boundary | Observable result |
| --- | --- | --- |
| 1 | [Participant worksheet](participant.md): alias, owned URL, starter SHA only | Reviewed non-secret initial commit, correct fetch/push destinations, owned default main |
| 2-3 | [L1-MANUAL](../src/workflows/learn-actions-manual.yml): whole file | Installed manual-only workflow; no push run, then explicit manual success |
| 4 | [L1-AUTO-EVENTS](../src/snippets/l1-auto-events.yml): `on` and all children, stopping before installed `permissions` | Bounded fresh push event without manual dispatch |
| 4-6 | [L1-AUTOMATIC-REFERENCE](../src/workflows/learn-actions-automatic.yml): comparison only | Same quality job, bounded events, PR/base and push/head distinction |
| 7 | [L1 format-fail fixture](../src/snippets/l1-format-fail.tf.txt): whole file replaces the working fixture | Valid HCL, actual `fmt -check` exit 3 |
| 7 | [Known-good fixture](../src/reference/main.tf.txt): comparison after local `fmt` | Repaired source and new green automatic run |
| 8 | [L1-ENVIRONMENT](../src/workflows/environment-gate.yml): whole file after environment configuration | Manual main-only gate waits, is reviewed, and prints a message |
| 9 | [Evidence worksheet](evidence.md), [instructor answers](instructor.md#knowledge-check-answers) | Real observations and explicit blocked/not-tested statuses |

Complete workflow templates are stored **only** under the source workflows directory. The event fragment is not a workflow; the reference fixture has a text suffix to prevent accidental execution. The published starter has no active workflow directory, nested repository, state, plans, credentials, or provider cache.
Publication of the instructor source and live GitHub rehearsal are separate main-agent activities. This package does not claim either occurred during focused authoring.
