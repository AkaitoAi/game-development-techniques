# Modular asset kit design

## Problem and design reasoning

Individually correct meshes can still form an unusable kit when their dimensions, connection rules, or visual density differ. Bethesda's *Skyrim* developers describe a kit as a reusable system that supports large-scale world building. They also identify obvious repeated details as a source of visual fatigue. Joel Burgess and Nathan Purkeypile revisited modular level production for shipped *Fallout 4*. The transferable inference is to validate an assembled space early and track the assembly contract alongside each mesh. [Joel Burgess and Nathan Purkeypile, Skyrim's Modular Approach to Level Design, GDC 2013 developer write-up](https://www.gamedeveloper.com/design/skyrim-s-modular-approach-to-level-design) · [Burgess and Purkeypile, Fallout 4's Modular Level Design, GDC 2016](https://www.gdcvault.com/play/1022930/-Fallout-4-s-Modular)

For mesh authoring fundamentals, read [3D modeling](3d-modeling.md). This guide focuses on the contracts between interchangeable environment pieces.

## Techniques and practical checks

### Establish scale and connection rules

Define the unit scale, placement grid, supported rotations, wall thicknesses, opening dimensions, and intended player clearances. Model a small set of floors, walls, corners, and openings before expanding the kit. Treat these as constraints for authoring and validation, with explicit exceptions for special pieces.

**Check** assemble a room, corridor, inside corner, outside corner, and vertical transition using the imported assets. Traverse them with the actual controller and camera. Measure clearance and record any placement requiring an unexplained offset or scale correction.

### Make pivots and sockets predictable

Choose a pivot convention for each piece family and name connection sockets by role and direction. Apply transforms consistently during export. Keep visual geometry, collision, and socket placement aligned in the asset's local space.

**Check** snap every compatible socket pair in each supported orientation. Compare socket position and axis differences against the project tolerance. Include mirrored pieces and a reimport. Verify the same transform aligns the visible surface and collision.

### Validate seams across rendering conditions

Give touching edges a consistent profile. Specify how normals, UV density, trim placement, and materials cross a boundary. Test the kit under representative light directions and distances, including transitions between detail levels.

**Check** build a strip of repeated pieces and inspect it at grazing angles, close range, and the longest supported view distance. Capture gaps, shading discontinuities, texture stretching, and detail-level transitions. Compare a wireframe view with the lit result to identify the responsible authoring layer.

### Budget variety at the assembled-space level

Provide variation where players notice repetition, including ends, junctions, landmarks, and recognizable surface details. Track how material choices and decorative variants affect draw cost, memory, collision, and authoring time. Keep the core placement contract compatible when adding visual variants.

**Check** ask a second designer to assemble two distinct spaces from the kit using its documented conventions. Record manual corrections, missing pieces, and time spent. Review repeated details in a traversal video and profile the assembled spaces before approving more variants.
