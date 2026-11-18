# Runtime mesh attribute updates

## Problem and design reasoning

Damage masks, ripples, growth, and deformation often change vertex data while triangle connectivity remains stable. Rebuilding the entire mesh can waste bandwidth and erase unrelated attributes. *Quake*'s released water-rendering code derives warped texture coordinates from prepared polygon data each draw. It demonstrates separating persistent geometry from time-varying surface inputs, though its immediate-mode implementation is historical. [id Software, Quake gl_warp.c](https://github.com/id-Software/Quake/blob/master/WinQuake/gl_warp.c)

Natalie Burke's GDC 2016 **Animating With Math** examines *Destiny* vertex animation, vertex-color control, and engine parameters used by shader functions. This supports choosing a shader evaluation when motion can be expressed from stable inputs. The CPU update and synchronization rules below are portable recommendations, rather than a description of Destiny's buffer implementation. [Burke, GDC 2016](https://gdcvault.com/play/1023249/Animating-With)

## Techniques and practical checks

### Choose data ownership before an update path

Specify whether the CPU, compute work, or vertex shader owns each changing channel. Use shader parameters for reconstructible visual motion, buffer updates for persistent per-vertex edits, and topology generation when connectivity changes. Keep gameplay collision and queries explicitly related to the visual deformation.

**Check** deform a mesh and query its visible surface with gameplay probes. Verify the declared collision policy and identify which representation is authoritative for hits, navigation, and saves.

### Update only the intended channels

Document channel format, stride, range, defaults, and vertex identity. Track dirty vertex ranges or chunks and merge nearby writes when beneficial. Preserve authored colors, UVs, and skinning data unless their ownership contract explicitly permits modification.

**Check** modify a small patch on a mesh with diagnostic values in every other channel. Compare untouched vertices and channels byte-for-byte where formats permit, and inspect the upload volume in a target-device capture.

### Publish coherent dependent data

Position edits may require updated normals, tangents, bounds, and motion-history data. Recompute only what the effect needs, and publish compatible versions together. Expand conservative bounds for shader displacement where exact bounds are unavailable.

**Check** apply the maximum deformation near a camera frustum edge with lighting and motion effects enabled. Look for premature culling, shading seams, stale shadows, and motion streaks against a reference capture.

### Respect asynchronous readers and mesh variants

Avoid overwriting data still consumed by rendering. Use the engine's supported update or buffering mechanism and measure synchronization stalls. Define how runtime edits transfer across LODs, instances, streaming reloads, and shared source meshes.

**Check** stress repeated edits while switching LODs, unloading and reloading the object, and rendering several instances. Confirm edits affect their intended owners, survive according to policy, and do not introduce recurring upload stalls.

See [Runtime mesh generation](runtime-mesh-generation.md) for topology changes and [Mesh vertex color workflows](mesh-vertex-color-workflows.md) for authored channel contracts.
