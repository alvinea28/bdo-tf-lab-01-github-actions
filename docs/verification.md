# Source verification - 2026-09-22

## Observed locally

- Terraform **1.16.3** formatting and provider-free validation passed.
- **3 complete workflow templates** passed actionlint **1.7.12**; the assembled automatic-event variant also passed. Optional ShellCheck/Pyflakes integrations were not run.
- The actual malformed-format sample returned **exit 3**. Formatting the isolated learner copy reproduced the supplied reference exactly; its subsequent check returned **0**. The published starter was not edited by this rehearsal.
- Guide command fences parse as Windows PowerShell 5.1. Parsing is not execution of remote Git/GitHub operations.
- Cross-package verification checked local links, syntax, LF-normalized whitespace, full Action pins, and absence of active workflows/state/plans/credential patterns. This is a bounded publication check, not a guarantee that arbitrary secrets cannot exist.

## Not claimed

No hosted GitHub Actions run, participant PR, environment approval, or live Azure operation was executed for these source checks. Lab 1 has no Azure workload or live application URL. Classroom timing and actual GitHub policies/reviewer behavior still need the instructor's rehearsal.

The instructor source is published on **dev**; learner copies establish **main**. Obtain the frozen source commit from the instructor or the repository's dev commit history. Keep expected learner outputs separate from observed results in [evidence.md](evidence.md).
