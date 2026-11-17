# Baked visibility and portals

## Shipped examples

- **Quake (1996).** The map compiler clipped visibility through chains of portals and stored a compressed potentially visible set (PVS) for each spatial leaf. At runtime, the renderer decoded the set for the viewer's leaf before walking eligible world geometry. [Michael Abrash, Quake postmortem](https://github.com/jagregory/abrash-black-book/blob/master/src/chapter-70.md)
- **Destiny (2014).** Bungie's GDC shader-system presentation identifies Umbra as the basis of visibility and describes using its visibility check for lights. Umbra's developer describes the middleware technique separately as runtime portal rasterization into a conservative depth buffer. [Bungie, GDC 2017 presentation](https://advances.realtimerendering.com/destiny/gdc_2017/Destiny_shader_system_GDC_2017_v.4.0.pdf) · [Umbra developer's technical overview](https://unity.com/blog/engine-platform/occlusion-culling-basics-unity-4-3)

## Problem and design reasoning

A frustum containing several rooms can submit geometry that is completely hidden by their dividing walls. Abrash documents trading preprocessing and storage for fast leaf rejection during play. The transferable inference is that stable, enclosed layouts offer more predictable savings than open terrain where most cells can see one another. Treat visibility data conservatively. An extra draw wastes time, while falsely rejecting a visible room breaks the frame.

## Techniques and practical checks

### Partition around durable occluders

Build or author cells around walls, floors, and rooms with well-defined openings. Connect adjacent cells using portal polygons, and register bounds spanning multiple cells in every relevant cell.

**Check** bypass culling and compare camera sweeps through thin walls, windows, and adjoining floors against the culled output. Inspect the cell and portal overlays where a visible object disappears.

### Bake conservative visibility

Compute reachability through portal chains while clipping against the accumulated viewing bounds. Store a conservative cell set and combine it at runtime with the current camera frustum. Track build time and data size separately from runtime savings.

**Check** a long corridor with staggered doorways and a large atrium with multiple connected levels. Verify zero false rejections, then compare submitted draws and CPU frame time with visibility disabled.

### Model moving openings explicitly

Bake visibility with movable doors open. Treat closed portals as runtime restrictions only where the closed geometry fully covers the opening. Keep a conservative fallback when cell data is missing or stale.

**Check** open, close, and partially open a door while crossing the threshold. Repeat with teleportation and late cell loading. Verify that visible geometry survives each transition.

### Validate every view independently

Resolve visibility for each gameplay, reflection, or spectator camera using its own position and projection. Give auxiliary views a conservative fallback when their origin sits outside authored cells.

**Check** use two cameras looking into different rooms and move one across a cell boundary. Compare each view against its own uncullled reference and profile the combined visibility cost.
