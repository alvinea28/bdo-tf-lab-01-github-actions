# Lab 1 known-good checkpoints

Return to [the participant guide](../../README.md) after comparing the relevant checkpoint.
These references are inactive. Do not create a second active quality workflow or recursively copy this directory into the Terraform root.

| Checkpoint | Known-good source | Use only after the exercise |
| --- | --- | --- |
| Manual installation | [L1-MANUAL](../workflows/learn-actions-manual.yml) | Compare the entire installed manual workflow; expect only dispatch before checkpoint 4 |
| Automatic event edit | [L1-AUTOMATIC-REFERENCE](../workflows/learn-actions-automatic.yml) | Compare the installed `on` block and unchanged job; keep only one active quality workflow |
| Format repair | [main.tf.txt](main.tf.txt) | Compare with the working fixture after running local `terraform fmt`; the deliberate [negative sample](../snippets/l1-format-fail.tf.txt) differs only in formatting |
| Manual approval | [L1-ENVIRONMENT](../workflows/environment-gate.yml) | Compare the separate gate after configuring `production`; source cannot copy environment settings |

`L1-AUTOMATIC-REFERENCE` is deliberately in the source **workflows** directory so all complete workflow templates live in one place.
The text-suffixed Terraform examples are not discovered by the active root. Never copy their text suffixes into that root as a substitute for editing the actual working fixture.
If a source comparison differs beyond the intended edit, inspect and repair the smallest change; preserve learner work, pinned versions, permissions, and organization rules.
