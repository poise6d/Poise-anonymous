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
