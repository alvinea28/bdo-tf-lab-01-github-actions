# Lab 1 - GitHub Actions without cloud access

**75 minutes.** Copy, inspect, edit, check, commit, push, observe, explain. Learn events, jobs, runners, steps, permissions, contexts, and environment approval.
Instructor source: [alvinea28/bdo-tf-lab-01-github-actions](https://github.com/alvinea28/bdo-tf-lab-01-github-actions/tree/dev), default branch **dev**. Your ordinary owned copy uses **main**; do not fork, mirror, initialize nested repositories, or push to the instructor.
This starter has **zero active workflows**. Templates are inert until copied at the stated checkpoint. No Azure account, Azure CLI, cloud credentials, backend, resources, plan, or apply is needed.
**Authoring status:** instructions are not evidence of live GitHub runs. Record your own observed results in [docs/evidence.md](docs/evidence.md); see [the authoring report](docs/daily%20work%20report/2026-09-22.md).

## Before starting

- Have Git, **Terraform 1.16.3**, VS Code, a browser, GitHub sign-in/push access, and an approved **empty owned public repository** with no README/license/gitignore initialization. No additional CLI or editor extension is required; Copilot is optional.
- Agree the source starter commit with the instructor. Public means non-secret training material only. Required environment reviewers work for public repositories on current GitHub plans; private/internal alternatives require suitable Enterprise support.
- Use **Windows PowerShell 5.1**, run each command separately, and stop on unexpected nonzero `$LASTEXITCODE`. The intentional formatting failure explicitly expects **3**. Workflow `run` blocks instead use **Bash on Ubuntu**, not PowerShell.
- Start in a parent folder, then keep every subsequent command at this clone's root. Save edits in VS Code before checking or committing. Git identity must already be configured; preserve existing author settings and authenticate through the normal Git/browser flow, never a token in a URL.
- Respect organization rules, required checks, review, and allowed Actions. If direct bootstrap pushes are prohibited, arrange an approved import/PR path with the instructor. Never force-push, disable rules, or add this intentionally filtered demo as a required check.

| Open when instructed | Purpose |
| --- | --- |
| [src/terraform/main.tf](src/terraform/main.tf), [versions.tf](src/terraform/versions.tf) | `L1-FIXTURE`: variable, local, output; exact CLI pin and no providers |
| [src/workflows/learn-actions-manual.yml](src/workflows/learn-actions-manual.yml) | `L1-MANUAL`: first complete workflow |
| [src/snippets/l1-auto-events.yml](src/snippets/l1-auto-events.yml) | `L1-AUTO-EVENTS`: replace only `on` |
| [src/workflows/learn-actions-automatic.yml](src/workflows/learn-actions-automatic.yml) | `L1-AUTOMATIC-REFERENCE`: compare after the trigger exercise |
| [src/workflows/environment-gate.yml](src/workflows/environment-gate.yml) | `L1-ENVIRONMENT`: separate manual, main-only message job |
| [src/reference/README.md](src/reference/README.md) | Recovery checkpoints; never a second active workflow |

## 1. Establish your owned copy (12 minutes)

Run from the parent folder; the clone command deliberately selects the instructor's **dev** branch.
```powershell
git --version
terraform version
git clone --branch dev --single-branch https://github.com/alvinea28/bdo-tf-lab-01-github-actions.git
Set-Location bdo-tf-lab-01-github-actions
git branch --show-current
git rev-parse HEAD
Test-Path .github/workflows
git branch -m main
```
**Expect:** Terraform `v1.16.3`, initial branch `dev`, an instructor-confirmed starter SHA, `False` for active workflows, then local `main`. Stop on a version/revision mismatch. Open this folder using **VS Code -> File -> Open Folder**.
Replace **both** placeholder strings below with your own approved owner/repository, not `alvinea28` or an instructor destination. First confirm the browser's empty-repository page and your ownership.
```powershell
$Owner = 'REPLACE_WITH_YOUR_GITHUB_OWNER'
$Repo = 'REPLACE_WITH_YOUR_EMPTY_LAB1_REPO_NAME'
if ($Owner -like 'REPLACE_*' -or $Repo -like 'REPLACE_*') { throw 'Replace both placeholders before continuing.' }
$OwnedUrl = "https://github.com/$Owner/$Repo.git"
git remote remove origin
git remote add origin $OwnedUrl
git remote -v
git remote get-url --all origin
git remote get-url --push --all origin
git ls-remote --heads --tags origin
```
**Expect:** exactly your HTTPS destination for **both fetch and push**; the last command succeeds with **no refs**. Any unexpected URL/history or authentication failure means stop, not force-push. Git copies history and source, **not** settings/environments.
Edit only the alias, owned repository URL, and starter SHA fields in [docs/participant.md](docs/participant.md), using non-secret values. Preserve the license and source history.
```powershell
git diff -- docs/participant.md
git add -- docs/participant.md
git diff --cached --check
git diff --cached --name-only
git commit -m "docs: identify my Lab 1 copy"
git push -u origin main
```
**Expect:** only the participant worksheet staged; GitHub **Code** shows your guide and worksheet on `main`. In **Settings -> Default branch**, verify `main` (locate this section under General if needed). **Actions** has no L1 run from this push. Record URLs/SHA, not credentials. Why did copying code not copy settings?

## 2. Install a manual-only quality workflow (8 minutes)

Starting on clean `main`, open [L1-MANUAL](src/workflows/learn-actions-manual.yml). Identify `workflow_dispatch`, job `quality`, runner `ubuntu-24.04`, named steps, `contents: read`, and the ten-minute job timeout.
**Optional Copilot explain:** select only the `on` through `permissions` blocks and ask: "Explain only this selected YAML: which event starts it and what token permission is granted? Do not edit, execute commands, push, or change GitHub/Azure settings. Explain why a push will not start it."
Accept only an explanation consistent with manual dispatch and read-only repository access; without Copilot, use the preceding sentence as the answer. No secrets or `id-token: write` belong here.
```powershell
git status --short
New-Item -ItemType Directory -Path .github/workflows -Force
Copy-Item src/workflows/learn-actions-manual.yml .github/workflows/learn-actions.yml
terraform '-chdir=src/terraform' fmt
terraform '-chdir=src/terraform' fmt -check
terraform '-chdir=src/terraform' init -backend=false -input=false -no-color
terraform '-chdir=src/terraform' validate -no-color
```
**Expect:** formatting check exits 0 (normally silent), initialization succeeds without providers/backend access, and `Success! The configuration is valid.` No Terraform plan/apply is part of this lab. Only the new active workflow should be untracked; caches are ignored.
Inspect the new active workflow in VS Code; it must match the source. All relative runner commands operate in the Terraform root, while checkout/setup action inputs are independent of that shell directory.
```powershell
git add -- .github/workflows/learn-actions.yml
git diff --cached --check
git diff --cached
git commit -m "ci: install manual Lab 1 quality workflow"
git push origin main
git rev-parse HEAD
```
**Expect:** **Actions** may now list **L1 - Learn Actions**, but this push produces **no run**. Clear UI filters and refresh; save the installation SHA. If a run exists, inspect its event and the actual committed `on` block, not an old run or another workflow.

## 3. Dispatch and read the manual run (6 minutes)

In **your repository -> Settings -> Actions -> General**, verify Actions are allowed and the pinned `actions/checkout` and `hashicorp/setup-terraform` are permitted. Keep repository token defaults read-only; no setting needs PR-write permission. If policy blocks an action, ask the owner rather than weakening policy.
Open **Actions -> L1 - Learn Actions -> Run workflow -> Use workflow from: main**. Enter non-secret note `manual-baseline`, then **Run workflow**. The workflow must already exist on the default `main` branch.
Open the new run -> **L1 cloud-free quality -> Report event, ref, and checked-out commit**, then the formatting, initialization, and validation steps. Inspect **Summary -> L1 run identity** too.
**Expect:** event `workflow_dispatch`, ref `refs/heads/main`, event/checked-out SHA equal to the installed main commit, blank PR-head SHA, and a green job. Record the run URL/SHA. This is a real manual run, not evidence of automatic triggering.
Notice that note/context values enter the shell through `env`, are quoted, and use a fixed `printf` format. The note is escaped for display, not evaluated; do not put expressions containing user text directly inside `run` scripts.
Recovery: if **Run workflow** is missing, verify default branch, destination, committed YAML, write access, and Actions policy. Fix only the cause; do not replace pins or add permissions. What is the runner, and what is a step?

## 4. Change only the event block, then prove a fresh push (7 minutes)

Open the active workflow installed in checkpoint 2. Select from top-level **`on:` through its children, stopping before `permissions:`**. Replace only that selection with the complete [L1-AUTO-EVENTS block](src/snippets/l1-auto-events.yml), starting at `on:`; keep the rest unchanged.
It retains manual dispatch, accepts pushes to `main` or `feature/**`, and accepts opened/updated/reopened PRs **targeting `main`**. It does not subscribe to all branches, tags, or schedules. Keep two-space YAML indentation and verify the diff.
**Optional Copilot improve:** select that new `on` block and ask: "Review only this selected event block. Suggest explanatory comments for push source-branch filtering versus PR base-branch filtering. Preserve every event, branch pattern, input, and indentation. Do not change jobs, permissions, action pins, or settings; do not execute commands or push."
Accept comment-only improvements; reject broader triggers, `pull_request_target`, write permissions, or cloud actions. No AI edit is required; the supplied snippet is the deterministic fallback.
```powershell
git diff -- .github/workflows/learn-actions.yml
git add -- .github/workflows/learn-actions.yml
git diff --cached --check
git commit -m "ci: enable bounded push and PR events"
git push origin main
```
The trigger-edit push can itself run. Now make a **fresh qualifying change**: in [docs/participant.md](docs/participant.md), change **Automatic push note** to `Fresh main push after enabling events` and save.
```powershell
git diff -- docs/participant.md
git add -- docs/participant.md
git diff --cached --check
git commit -m "docs: prove a fresh automatic main push"
git push origin main
git rev-parse HEAD
```
**Expect without clicking Run workflow:** a new successful run for this newest SHA, event `push`, ref `refs/heads/main`. Save it separately from the trigger-edit run. YAML defines events; Settings permits execution. There is **no automatic-trigger toggle** in Settings.
Afterward compare with [the completed automatic reference](src/workflows/learn-actions-automatic.yml); do not install that reference as a second workflow. If no run, inspect committed `on`, branch, Actions policy, and UI event filters before retrying with a new qualifying commit.

## 5. Observe a feature-branch PR (7 minutes)

From clean `main`, create the branch below, then change only **PR note** in [docs/participant.md](docs/participant.md) to `PR event observed`.
```powershell
git switch -c feature/l1-review
```
Save the worksheet and publish the reviewed change.
```powershell
git diff -- docs/participant.md
git add -- docs/participant.md
git diff --cached --check
git commit -m "docs: exercise the PR event"
git push -u origin feature/l1-review
git rev-parse HEAD
```
In your GitHub repository select **Pull requests -> New pull request**, base **your main**, compare **your feature/l1-review**, then **Create pull request**. Do not open it against the instructor. Inspect the PR's **Checks** and the matching Actions run.
**Expect:** the branch push and PR can create **two** quality runs. The PR run says `pull_request` and `refs/pull/<number>/merge`; its event/checkout SHA is normally a synthetic merge commit, while **PR head SHA** equals your local commit. Record both SHAs, not a false equality.
Leave the filtered L1 check **non-required**. Preserve all existing required checks/reviews. An author cannot approve their own PR; use an eligible peer if required. After current checks/rules pass, **Merge pull request -> Confirm merge** and synchronize:
```powershell
git switch main
git pull --ff-only origin main
```
**Expect:** the merged worksheet on local `main`; no environment approval or Azure activity. If merging is blocked, resolve the stated rule with the instructor instead of using an administrator bypass.

## 6. Pair a filter negative with a positive control (7 minutes)

Start on clean, synchronized `main`. Create a **nonmatching** branch; do **not** open a PR from it (a PR to main would match the separate PR filter).
```powershell
git switch -c demo/l1-no-push
```
Change only **Filter negative note** in [docs/participant.md](docs/participant.md) to `Nonmatching push`, then save.
```powershell
git diff -- docs/participant.md
git add -- docs/participant.md
git diff --cached --check
git commit -m "docs: observe a nonmatching branch push"
git push -u origin demo/l1-no-push
git rev-parse HEAD
```
**Expect:** Code shows that exact remote commit, but Actions has **no L1 quality push run** for its SHA. Refresh with filters cleared; record observation time, committed `on` block, enabled Actions, and the SHA. Absence alone is not proof of a working filter.
Create a matching branch from this same revision; keep all workflow source unchanged. Change only **Filter positive note** to `Matching feature push`.
```powershell
git switch -c feature/l1-filter-positive
```
Save, inspect, and push a fresh positive commit.
```powershell
git diff -- docs/participant.md
git add -- docs/participant.md
git diff --cached --check
git commit -m "docs: prove the matching filter control"
git push -u origin feature/l1-filter-positive
git rev-parse HEAD
```
**Expect:** a successful `push` run on `refs/heads/feature/l1-filter-positive`. Record its URL/SHA beside the negative. Open a PR from this branch to your `main`, wait for its PR run, and **leave it open** for checkpoint 7. Why would a PR from the negative branch still run?

## 7. Cause a real formatting failure, then repair it (10 minutes)

Stay on `feature/l1-filter-positive` with its PR open and green. Open [the deliberate negative sample](src/snippets/l1-format-fail.tf.txt): valid HCL with bad spacing only. It replaces the fixture, not the version pin; the source snippet must stay outside the active Terraform root.
```powershell
Copy-Item src/snippets/l1-format-fail.tf.txt src/terraform/main.tf
terraform '-chdir=src/terraform' fmt -check
$LASTEXITCODE
```
**Expect:** a file name and exit **3**. Do **not** format it yet. If exit 0, verify the saved destination and directory; do not manufacture a failure screenshot. Deliberately commit only this formatting defect to the learning PR:
```powershell
git diff -- src/terraform/main.tf
git add -- src/terraform/main.tf
git diff --cached --check
git commit -m "test: demonstrate Terraform format failure"
git push origin feature/l1-filter-positive
git rev-parse HEAD
```
**Expect:** fresh automatic push/PR runs fail specifically at **Check Terraform formatting**, with exit 3. Later initialization/validation steps are skipped. Save the actual failed run URL/SHA/log; do not merge or disable a check to conceal it. This failure cannot deploy anything.
Repair locally using Terraform itself, then compare [the known-good fixture](src/reference/main.tf.txt) if needed. Do not use recursive formatting across the negative snippets.
```powershell
terraform '-chdir=src/terraform' fmt
terraform '-chdir=src/terraform' fmt -check
terraform '-chdir=src/terraform' validate -no-color
git diff -- src/terraform/main.tf
git add -- src/terraform/main.tf
git diff --cached --check
git commit -m "fix: restore canonical Terraform formatting"
git push origin feature/l1-filter-positive
git rev-parse HEAD
```
**Expect:** local exit 0 and a **new green automatic run for the fix SHA**, not a rerun of the broken commit. Record failed/fixed evidence. Merge the repaired PR only after current checks and any required review pass, then `git switch main` and `git pull --ff-only origin main`.

## 8. Add a separate main-only environment approval (13 minutes)

This is a GitHub **deployment environment**, not YAML `env` or a Terraform workspace. Its name `production` matches the source sample but represents only a **message-only workshop gate**, not real production.
In **your repository -> Settings -> Environments -> New environment**, enter exactly **production**, then **Configure environment**:
1. Under **Deployment protection rules -> Required reviewers**, select your participant GitHub account as an eligible reviewer (or the assigned reviewer); save the rule.
2. Leave **Prevent self-review unchecked/OFF**. This deliberately permits a listed initiator to approve this sandbox deployment; it does **not** permit self-approval of a PR.
3. Under **Deployment branches and tags**, choose **Selected branches and tags**, add a **Branch** rule matching exactly **main**, and no tag or wildcard rules. Save. Do not use a broad **Protected branches only** substitute.
4. Do not configure environment/repository secrets, OIDC, or Azure settings. Do not bypass protection; if required reviewers are unavailable or policy forbids this configuration, **stop and record the blocked gate** rather than running it unprotected.
Starting on clean synchronized `main`, open [L1-ENVIRONMENT](src/workflows/environment-gate.yml). Inspect manual-only `on`, the main guard, the dependent job's `if`, and fixed environment `production` before copying.
```powershell
git switch -c feature/l1-environment
Copy-Item src/workflows/environment-gate.yml .github/workflows/environment-gate.yml
git add -- .github/workflows/environment-gate.yml
git diff --cached --check
git diff --cached
git commit -m "ci: add a cloud-free environment gate"
git push -u origin feature/l1-environment
```
Open a PR to your `main`. **Expect:** only the existing quality workflow runs automatically; PRs must **not** request `production`. Obtain all current required checks/reviews, merge through the normal path, then synchronize:
```powershell
git switch main
git pull --ff-only origin main
git rev-parse HEAD
```
Open **Actions -> L1 - Environment gate -> Run workflow -> Use workflow from: main**, note `approve-learning-only`, then **Run workflow**. Do not choose the feature branch or click **Re-run** on an older run.
**Expect before approval:** **L1 require manual main** succeeds; **L1 approved cloud-free message** waits for `production` review. Record run URL, main SHA, and the pending gate before approving. If the message already ran, protection was missing: record the miss, fix the environment, and dispatch a fresh run; do not claim an approval occurred.
As the eligible reviewer open that run -> **Review deployments -> production -> Approve and deploy**. Record reviewer and approval time from the UI, not from `github.actor` (the initiator is not proof of the approver).
**Expect after approval:** a green message job saying no Azure login/plan/apply/resource operation ran; the run summary identifies `workflow_dispatch`, `refs/heads/main`, and this main commit. Wrong-ref dispatches are rejected by the guard before requesting approval. Approval releases a job; it does not add a new trigger.

## 9. Explain, capture evidence, and finish (5 minutes)

The upstream drift schedule is **`41 3 * * *` = 03:41 UTC daily**, not local time; schedules run the default branch, may be delayed, and require committed workflow source. Read [the pinned upstream drift workflow](https://github.com/Azure-Samples/terraform-github-actions/blob/2e6dee79491254d4eea193b1691d2c85fd32af0f/.github/workflows/tf-drift.yml) only; **do not activate a schedule here**.
Fill [docs/evidence.md](docs/evidence.md) with actual URLs/events/refs/SHAs and pending/approved observations. No Azure evidence is applicable, and local validation is not proof of GitHub execution. Avoid tokens, personal secrets, raw state, or sensitive screenshots.
Commit only the completed evidence through a fresh feature branch and PR; do not mix it into an older failed run:
```powershell
git switch -c feature/l1-evidence
git diff -- docs/evidence.md
git add -- docs/evidence.md
git diff --cached --check
git commit -m "docs: record observed Lab 1 evidence"
git push -u origin feature/l1-evidence
```
Open/merge its PR only after current checks/rules pass. Retain source, failed/fixed runs, and approval evidence until the instructor's deadline. There are **no cloud resources to destroy**; do not run `terraform destroy`. Optional after evidence: **Actions -> workflow -> ... -> Disable workflow** for each of the two demos, without changing required protections or deleting others' runs.
Knowledge check: (1) Why did the initial push not run? (2) Which file edit enables automation? (3) What does the PR branch filter test? (4) Why can PR and local SHAs differ? (5) How was the filter negative proven? (6) Does `env` create an approval? (7) Who may approve this deployment versus the PR? (8) What was actually deployed?
Discuss first; then compare [instructor answers](docs/instructor.md#knowledge-check-answers). Completion requires all evidence rows, or an explicit blocked/not-tested status, never an invented pass.

## Troubleshooting without bypasses

| Symptom | Check and bounded recovery |
| --- | --- |
| Wrong remote or nonempty destination | Stop before push; recheck both URLs/ownership and agree a clean empty destination. No force/mirror push. |
| Push denied or PR cannot merge | Use the normal authenticated Git flow; inspect organization rules/current required checks/reviewer access. Keep protections. |
| Manual button missing | Workflow must be committed on default `main`, contain `workflow_dispatch`, be enabled, and caller must have write access. |
| No automatic run | Check committed `on`, remote SHA, event, source/base branch filter, Actions policy, and UI filters; repeat a fresh matching control. |
| Expected no run but a run appeared | Inspect event: a PR to `main` matches even when its head is `demo/**`; an older/manual/other-workflow run is not this push. |
| YAML/action policy error | Compare the selected block with the supplied reference; preserve spaces/pins and ask the owner about blocked Actions. |
| Terraform version/format error | Verify `v1.16.3` and root path; use `fmt` only to repair checkpoint 7, never remove the check. |
| No waiting gate or reviewers unavailable | Verify exact environment, required reviewer rule/plan support, and main-only branch rule. Stop if unsupported; never replace with an unprotected job. |
| Reviewer cannot approve | Verify listed reviewer identity and Prevent self-review OFF if initiator; organization rules win. Do not use an admin bypass. |

See [source attribution and exact pins](docs/source-alignment.md), [instructor preparation](docs/instructor.md), and the [MIT license](LICENSE). No GitHub CLI, Azure CLI, or cloud operations are required for this lab.
