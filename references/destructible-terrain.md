# Destructible terrain and volumetric editing

## Problem and design reasoning

Use volumetric edits when players need to carve tunnels, leave an overhang, or add material underneath an existing surface. A single height value per horizontal sample cannot represent both a cave floor and its ceiling. Treat an edit as a change to the world representation, with render meshes, collision, and navigation derived from the changed region. See [Destruction](destruction.md) for breakable actors and debris.

## Shipped examples

- **Red Faction (2001).** Volition physics programmer Tim Schroeder describes GeoMod changing level geometry at runtime and the collision problem that changing geometry created. The article contrasts this with precomputed spatial partitioning, whose assumptions change when surfaces are added or removed. [Schroeder, *Collision Detection Using Ray Casting*, Game Developer, August 2001](https://media.gdcvault.com/GD_Mag_Archives/GDM_August_2001.pdf)
- **Astroneer (2019).** System Era presented free-form sculpting as a defining feature at GDC Summer. The studio's terrain-tool write-up explains its design goal of encouraging creative problem solving and shows how augments turn one system into tools for tasks such as selective removal. Brendan Wilson describes revising the voxel system after Early Access to give artists more control over planetary environments. A transferable inference is that editing rules and world authoring deserve separate tests because one determines what players can change and the other determines where those changes are useful. [System Era's GDC Summer 2020 presentation](https://media.gdcvault.com/gdcsummer2020/presentations/Biddlecom-Aaron-MiningYourOwnDesign.pdf) · [Terrain tool augments](https://blog.astroneer.space/p/augments/) · [Brendan Wilson's GDC livestream interview](https://www.gamedeveloper.com/design/what-i-astroneer-i-s-devs-learned-while-leaving-early-access)

## Techniques and practical checks

### Use one canonical terrain state

Represent the editable volume with occupancy cells or a sampled density field, depending on the desired surface style. Store material information alongside the state used to decide whether a sample is solid. Derive render and collision surfaces from the same revision. If their updates cannot commit in the same frame, define when the new space becomes traversable and how that is signaled.

**Check** carving a tunnel beneath an intact roof, then casting rays and walking through it. Compare what players see, where they can stand, and what a subsequent brush hit selects before and after collision updates.

### Bound the region each brush invalidates

Convert the brush footprint into dirty chunks and include neighboring samples needed for shared edges and normals. Coalesce overlapping work and tag results with their input revision to discard a mesh or collider generated from an older edit.

**Check** a brush stroke along a chunk seam, at a four-chunk corner, and at a level-of-detail boundary. Look for cracks or stale collision and record queued chunk work, memory peaks, and frame time during a long stroke.

### Define edit permissions and traversal consequences

Decide which volumes are protected, which materials can be added or removed, and which tasks may depend on a path remaining open. Reassess navigation and entity support only where the affected area intersects them. Provide a recovery route for required gameplay that can be isolated by legal edits.

**Check** around a mission exit, the edge of protected terrain, and directly underneath an actor. Confirm the intended constraints, safe contact behavior, and reachability after the queued updates complete.

### Persist changes against a versioned base

Save bounded edits or changed chunk data with the base world identifier and a version. If edits come from several networked players, establish an authoritative order and provide refreshed chunk state for clients joining midway.

**Check** overlap two edits to the same chunk, then verify authoritative terrain after saving, reloading, leaving the streaming region, and joining a live session late where supported.
