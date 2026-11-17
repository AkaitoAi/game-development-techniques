# 3D modeling for games

## Problem and design reasoning

**Problem.** A mesh looks convincing in an editor but collapses at a joint, clips the player, or requires expensive manual rework after export.

**Design reasoning.** Naughty Dog documented asset volume and detail as reasons for a dedicated vehicle pipeline. The same trade-off applies to interactive props and characters. Evaluate topology and export in the context of what the asset must do in-game.

## Shipped examples

**Quake (1996).** The released tools describe a model compiler that combines a skin image with 3D animation frames to make the runtime model. The source generates texture coordinates and stores a seam flag. This illustrates a pipeline with an explicit handoff from authoring inputs to a compact runtime asset. [id Software's tool documentation](https://github.com/id-Software/Quake-Tools/blob/master/qutils/README.TXT) · [Model compiler source](https://github.com/id-Software/Quake-Tools/blob/master/qutils/MODELGEN/MODELGEN.C)

**The Last of Us (2013).** Naughty Dog's Judd Simantov presented the modeling and rigging of the game's major characters together. The modeling work could therefore be evaluated against actual facial and body articulation as well as the neutral pose. [Judd Simantov's Autodesk presentation](https://www.youtube.com/watch?v=myZcUvU8YWc)

**The Last of Us Part II (2020).** Naughty Dog's Matthew Trevelyan Johns documented how the volume and detail of vehicle assets motivated a dedicated production pipeline. His GDC slides include mesh topology and explain where the workflow removed a stage of outsource production. Model architecture can determine how much effort is required to make or revise a family of props. [Naughty Dog vehicle pipeline, GDC 2021](https://media.gdcvault.com/GDC%2B2021/33713_GDC-2021-MatthewTJohns-ND.pdf)

## Techniques and practical checks

### Block out the model in its playable context

Set the origin, handedness convention, up axis, unit scale, and any attachment anchors before polishing. Block out doors, handholds, seats, and collision with simple volumes. Review the silhouette under the gameplay camera and keep interactive features large enough to read at their working distance.

**Check** the exported blockout against a player-sized reference. Enter, climb, grab, and pass through any supported part. Compare render and collision silhouettes at the closest and farthest gameplay distances. Reimport after a source edit and verify the pivot and anchors remain stable.

### Place topology for deformation and outline

Spend geometry on visible curves and joints that stretch or fold. For a skinned model, bind a prototype before finalizing the topology. Inspect edge flow with representative joint angles and layer worn or held items over the moving mesh. For a rigid prop, place edges where silhouette or shading changes and align separate parts with their intended hinges.

**Check** maximum intended joint angles and moving parts from multiple cameras. Look for pinching, changing normals, and large silhouette jumps. Compare exported vertex counts with authoring counts because UV, material, and normal splits can add runtime vertices.

### Transfer detail selectively

Keep detail that changes the silhouette in geometry. For fine detail on an otherwise suitable surface, bake from a high-detail source onto the target UV layout. Use projection bounds that reach the intended source without catching neighboring parts. Bake with the same tangent convention and triangulation used at runtime.

**Check** seams, thin parts, and mirrored geometry under rotating directional light at close range and several mip levels. Compare with the source mesh. Bake one asset again after export and confirm its result stays consistent.

### Simplify for screen size and workload

Build levels of detail around screen coverage and expected instance counts. Preserve interaction anchors, dominant shapes, and openings at the distances where they stay visible. Reduce material sections along with triangles when draw submission limits performance. Document which geometry controls collision and which geometry renders.

**Check** an orbit and a slow approach through each transition, then profile a scene containing many copies. Watch silhouette, shadows, deformation, and attachment alignment. Record visible geometry and CPU and GPU time so the team can identify which budget improved.
