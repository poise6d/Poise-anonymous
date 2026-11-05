# Release-candidate acceptance

The acceptance suite imports the candidate's layered packages into an empty data
workspace, uses a fresh source checkout, and runs each of the four reference
policies, small from-scratch training and checkpoint continuation. The original
workspace and research checkouts are hidden from simulator/optimizer processes
with a filesystem namespace. The prepared Isaac Lab runtime is reused: this
validates the documented prerequisite, not installation of Isaac on a new OS.

The optional LyGra check fetches pinned upstream sources, applies the recorded
patch, verifies CUDA binaries and executes a small fresh generation/augmentation/
physics-filter chain. It uses a separate optimizer Python environment. A small
chain checks integration, not the formal generation quota or coverage quality.

Machine-readable results are written during the acceptance run. No full 192k
training run, fresh operating-system installation or real-robot test is implied.
The historical wrist success-rate benchmark used model_35600; it is not a score
for the fixed model_35800 or model_89800 reference candidates.

## Anonymous review validation: 2026-11-05

All four reference policies passed 64-step headless rollout. Each setup also
passed two fresh PPO updates, checkpoint saving, two resumed updates, counter
continuation and finite-weight checks. Tests passed: 190, with 2 skips and
6 subtests. Tests used a fresh local Git clone with the original development
workspace inaccessible. Final packages passed dependency/hash checks, deep
asset checks and repeat-import reuse for every setup.

A fresh pinned LyGra installation in a separate optimizer environment passed
its CUDA checks and the complete search, preload-command, augmentation and
physics-filter chain. Search yielded 11 candidates; 9 passed initial filtering.
Two parents produced 32 perturbed candidates; 30 passed the second filter.
The final smoke cache has 12 states, six per parent. The chain took 115.5 seconds.
This small check is not a generation-coverage or full-scale timing guarantee.

The four checkpoint files and all numerical asset values are preserved.
Only historical paths in ten grasp caches and the palm SDF metadata were
redacted; the dependent hashes were rebound. Runtime Python code is unchanged.
Third-party licenses retain their original bytes. Prepared Python 3.11 / Torch
2.7 cu128 / Isaac Sim 5.1 / Isaac Lab 0.48 and Python 3.9 / Torch 2.8 cu126
optimizer dependencies were reused on an RTX 4090 reporting 49,140 MiB VRAM.

See the [machine-readable validation receipt](validation-2026-11-05.json).
These results do not imply a new 192k training run, GUI or hardware acceptance.
Hosted CI and anonymous HTTPS downloads require the repository and Release to
be uploaded; they are not part of this local validation.

## Repeat the check

Create a new source clone and put the candidate packages outside the original
workspace. On Linux with bubblewrap and a prepared simulator environment:

```bash
python scripts/release/acceptance.py \
  --checkout /tmp/poise-clean \
  --assets /tmp/poise-assets \
  --python /path/to/isaac-environment/bin/python \
  --hide-workspace /path/to/original-workspace \
  --output /tmp/poise-acceptance
```

The checkout, package directory, Python environment and output must be outside
the directory being hidden. The output directory must be new. Run the independent
optimizer check using [the generation smoke profile](../new_objects.md#release-generation-assets).
