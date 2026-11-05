# Anonymous review snapshot

This repository provides the Poise implementation, environment recipes, four
reference policies and reproduction instructions for anonymous review.
The project page is https://poise6d.github.io/.

## Reproduction

Follow [installation](installation.md) and [reproduction](reproduction.md).
Training and reference rollout require the Isaac Sim / Isaac Lab environment.
The supplied caches allow these operations without installing LyGra. New grasp
generation uses the separate LyGra environment; robot deployment uses the
separate FoundationPose environment and hand SDK.

The four fixed setups preserve their training settings and reference policy
weights. They provide runnable reference experiments; they should not be treated
as exact reproductions of every experiment or aggregate statistic in the paper.
See each [baseline record](baselines/README.md) for scope and evaluation limits.

## Snapshot scope

- Prior repository history, author identities, personal links and identifying
  source-history provenance are omitted.
- The training, inference, grasp-generation and deployment Python implementations
  are preserved. No algorithm, reward, success criterion or numerical training
  parameter is changed for anonymity.
- Historical workstation paths in ten grasp caches and the palm SDF metadata
  are redacted. Numerical arrays and grasp rows are unchanged. All four reference
  checkpoint files are unchanged, including policy and optimizer tensors.
- Asset hashes and dependent configuration hashes identify these sanitized files.
  Shared packages are hosted under the anonymous account; downloads do not
  redirect through an author-owned project repository.
- Third-party copyright notices, licenses and upstream dependency pins remain.
  Hardware and dependency names describe the required technology, not authorship.

Review resources are packaged under `v0.1.0-rc1`. An offline copy can be installed
with `poise fetch-assets --setup <setup-id> --source /path/to/assets --with-reference`.
The final manuscript will provide the permanent repository and citation.

Validation procedure and results are recorded in [release validation](release/validation.md).
