# Game asset scale conventions

## Problem and design reasoning

An asset can appear correct by itself while its import conversion makes doors too narrow, animation steps too long, physics bodies unstable, or modular pieces impossible to join. Joel Burgess's *Skyrim* presentation treats modular level design as a production system for building large amounts of playable space. Ubisoft describes using interchangeable kits for *Assassin's Creed Odyssey*. Those shipped workflows depend on pieces that can be assembled predictably. A transferable inference is to agree on unit conversion, orientation, pivots, and gameplay dimensions before producing a large asset set. [Joel Burgess, Modular Level Design for Skyrim, GDC 2016](https://media.gdcvault.com/gdc2016/Presentations/Burgess_Joel_Modular%20Level%20Design.pdf) · [Ubisoft, Assassin's Creed Odyssey: The Game That Greece Built](https://www.ubisoft.com/en-us/game/assassins-creed/news/8bMNzF2KQAwevlh8VrQVj/assassins-creed-odyssey-the-game-that-greece-built)

## Techniques and practical checks

- Specify the authoring unit, runtime unit, forward/up axes, handedness, and import conversion for each asset class. Keep a measured reference object and representative character in an integration scene. **Check** export and reimport a known-size cube and character using each supported tool path. Measure runtime bounds and verify orientation, collider size, and camera framing.
- Give modular pieces explicit connection planes, snap increments, and pivot conventions. Distinguish visual trim from structural dimensions and include a plan for closing unmatched edges. **Check** build a closed room, stairs, and a rotated junction using only the declared placement rules. Inspect gaps and overlaps from gameplay distance and verify traversal clearance.
- Apply scale changes through the full dependency chain. Audit collision shapes, joint anchors, root displacement, particle sizes, audio distances, and interaction reach when changing the model or its parent scale. **Check** resize a representative asset and exercise its physics, animation, interaction, and effects. Compare those outcomes against authored dimensions rather than visual appearance alone.
- Validate imports and record intentional exceptions. Use bounds, transform, and socket checks that identify the offending asset and expected convention. **Check** introduce a known unit mismatch, rotated pivot, and nonuniform scale in disposable fixtures. Confirm validation catches each at the intended stage, then package the corrected fixtures and verify the same dimensions in a player build.

See [Modular asset kit design](modular-asset-kit-design.md), [3D modeling](3d-modeling.md), and [Game transform hierarchies](game-transform-hierarchies.md).
