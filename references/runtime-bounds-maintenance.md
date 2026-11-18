# Runtime bounds maintenance

## Problem and design reasoning

A render instance can have the right transform and still disappear because its culling bounds describe an earlier pose or a different representation. Bounds also feed visibility, shadows, and streaming, so overly large bounds waste work while undersized bounds hide valid geometry. In the released *Quake III Arena* renderer, MD3 culling combines the old and new frame bounds before testing a local box. This is a concrete example of accommodating geometry represented between animation frames. [id Software, MD3 culling implementation](https://github.com/id-Software/Quake-III-Arena/blob/master/code/renderer/tr_mesh.c)

Ubisoft's shipped *Assassin's Creed Unity* renderer uses mesh clustering to make GPU culling effective within heavily batched scenes. Ulrich Haar and Sebastian Aaltonen's SIGGRAPH talk documents the density and CPU constraints behind that choice. The revisioned bounds publication below is an engine-independent recommendation motivated by such culling dependencies. [Ubisoft, GPU-Driven Rendering Pipelines, SIGGRAPH 2015](https://advances.realtimerendering.com/s2015/index.html)

## Techniques and practical checks

### Match bounds to the representation

Derive bounds from the mesh, pose, deformation range, and LOD that can render during the frame. For a local axis-aligned box transformed by an affine matrix, transform its center and calculate new extents using the component-wise absolute linear matrix. Include finite checks and a defined policy for procedural displacement.

**Check** transform all eight box corners through a fixture containing rotation, nonuniform scale, reflection, and a rotated parent. Assert every transformed corner lies inside the computed world box within the chosen numeric tolerance.

### Publish transforms and bounds together

Associate geometry, transform, deformation, and bounds with one render revision. Complete required bounds work before visibility consumers read that revision. Discard delayed results computed for a retired mesh, actor generation, or older edit.

**Check** delay a bounds job, replace the mesh with a larger one, and release the old job. Assert the obsolete result cannot overwrite the replacement bounds. Sweep the camera across the frustum edge and check visibility against a conservative reference.

### Expand for intermediate motion deliberately

A union of endpoint bounds can cover linear interpolation of bounded geometry. Rotations and nonlinear deformation may require additional samples or an analytic envelope. Use the sweep relevant to each consumer, and reset history at discontinuities such as teleports.

**Check** rotate a long thin object through 180 degrees while its endpoints occupy the same box orientation. Sample intermediate geometry and assert the motion envelope contains it. Repeat a teleport and confirm the old travel path does not retain visibility or streaming demand indefinitely.

### Measure conservative error

Record the source and age of bounds and visualize them alongside the rendered geometry. Compare culled frames with a controlled run that disables the relevant culling stage. Treat false negatives as correctness failures and excess retained work as a performance measurement.

**Check** replay an animation and camera route through visibility boundaries with culling enabled and disabled. Count missing visible samples, shadow discontinuities, and retained instances, and measure the culling and rendering cost on the same build.

See [Runtime render instance management](runtime-render-instance-management.md) for instance publication and [Animation runtime LOD](animation-runtime-lod.md) for representation changes.
