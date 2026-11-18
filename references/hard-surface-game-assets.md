# Hard-surface game assets

## Problem and design reasoning

A mechanical prop may look detailed in a modeling viewport while its silhouette, highlights, material boundaries, or moving clearances fail in the game. Repeating bespoke geometry and texture work across a large asset family can also make revisions expensive. This guide focuses on rigid manufactured objects. [3D modeling](3d-modeling.md) covers the broader asset handoff.

For *The Last of Us Part II (2020)*, Naughty Dog identified the number, detail, and narrative importance of vehicles as reasons to revise its creation pipeline. Matthew Johns explains the memory and production limits of the earlier baking workflow and a more scalable approach. [Naughty Dog, Driving Innovation: A New Vehicle Pipeline for The Last of Us: Part II, GDC 2021](https://media.gdcvault.com/GDC%2B2021/33713_GDC-2021-MatthewTJohns-ND.pdf)

For *Killzone Shadow Fall (2013)*, Guerrilla describes adopting physically based asset creation to improve efficiency, quality, and flexibility under art direction. This supports evaluating manufactured surfaces in the intended renderer and lighting. The modeling checks below are recommended applications, rather than an attribution of a particular bevel or topology recipe to Guerrilla. [Guerrilla, Lighting Killzone Shadow Fall](https://www.guerrilla-games.com/read/lighting-killzone-shadow-fall)

## Techniques and practical checks

### Allocate detail to the gameplay view

Block out primary masses and the functional silhouette at the asset's real scale. Establish expected camera distance and the parts players inspect or interact with. Allocate geometry to silhouette changes and visible highlights, then use the project's material or baked-detail workflow for smaller surface features.

**Check** compare the blockout and detailed asset from the nearest and typical gameplay views. Inspect thin features during motion and at the lowest supported quality. Verify the added detail remains visible and that important interactive parts are identifiable.

### Make shading topology explicit

Choose where an edge should appear sharp, rounded, or continuous. Keep triangulation, vertex normals, tangent generation, and UV seams consistent through export and baking where the workflow depends on them. Use the actual runtime mesh to check the bake.

**Check** rotate the imported asset under a broad light and a small bright source. Inspect curved panels, mirrored parts, hard edges, and UV seams for changing highlights, dark wedges, and bake artifacts. Repeat after the mesh is triangulated and reimported.

### Separate reusable surfaces from unique identity

Choose which materials, trim sections, panels, and wear treatments can be shared across the asset family. Reserve unique texture space for recognizable marks or story detail that players can actually inspect. Keep material scale consistent when pieces are resized or reused.

**Check** assemble several variants together under the same light. Compare texel density, roughness scale, visible repetition, draw cost, and texture residency. Change a shared surface and verify the update across every intended consumer.

### Validate moving parts and gameplay collision

Define pivots, attachment points, allowed travel, and collision shapes for doors, wheels, lids, and other articulated parts. Check both visible clearance and the collision behavior needed by the game. Keep these contracts with the asset rather than relying on scene-specific corrections.

**Check** run the complete motion range in a representative scene, including interaction from both sides and contact with the player. Swap an asset variant and verify attachments, collision, occlusion, and closed-state alignment still work.
