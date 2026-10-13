# S6: healthy local Git baseline

- **Observed scope:** Kind GitOps exercise S6, local bare Git source.
- **Value:** baseline commit **44cf25f** restored the intended healthy configuration before the S6 bad-config injection.
- **Time:** recorded in the S6 evidence run on **2026-09-22 UTC**.
- **Boundary:** the healthy baseline was a Git revision; the later bad config was introduced by **kubectl patch**.

Source type/name: gitops-baseline-local-git.txt; observation: S6 run dated 2026-09-22 UTC; SHA-256 prefix: 4d7f1a97b0f9.
Source type/name: gitops-canary-runtime.json; observation: verified 2026-09-22 00:34:33 UTC; SHA-256 prefix: a7c859bdf84d.

[Next: injected change](02-bad-config-injection.md).
