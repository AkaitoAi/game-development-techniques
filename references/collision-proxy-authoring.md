# Collision proxy authoring

## Problem and design reasoning

The visible mesh may contain thin trim, small cavities, or many triangles that contribute little to a useful physical contact. An over-simplified proxy can block an apparently open route or let objects penetrate a visible surface. The proxy needs to express the intended contacts within the solver's supported shape types.

In shipped *Diablo III*, Erin Catto's GDC 2012 talk connects ragdoll shape and inertia choices to how bodies settle. It also describes simulating a subset of skeleton bones. This supports evaluating the collision representation by its physical behavior and cost. The released *Quake III Arena* collision loader separately reads planes, brush sides, brushes, contents, and bounds from its map data. The authoring contracts below are portable recommendations informed by these examples. [Erin Catto, Diablo 3 Ragdolls, GDC 2012](https://box2d.org/files/ErinCatto_Ragdolls_GDC2012.pdf) · [id Software, Quake III Arena collision loader](https://github.com/id-Software/Quake-III-Arena/blob/master/code/qcommon/cm_load.c)

## Techniques and practical checks

### Author for the intended interaction

List which surfaces must support a character, stop a projectile, hold another object, or remain passable. Choose primitives, convex parts, or supported triangle geometry for those tasks. Preserve important openings and avoid spending contact complexity on invisible detail.

**Check** traverse the asset with the real player capsule, place a representative prop on it, and test the intended query types. Inspect a doorway, thin railing, ledge, and cavity. Compare blocked and passable regions with the authored intent.

### Fix the import contract

Record units, local origin, transform rules, shape type, and collision purpose alongside the asset. Apply the same conversion to visual and collision geometry. Validate degenerate faces, unsupported concavity, inverted scale, and detached proxy parts during import.

**Check** import a known-size fixture under a rotated parent and supported nonuniform scale. Visualize the proxy and measure its position, clearance, and dimensions. Reject unsupported transforms with an asset-specific message.

### Measure physical behavior

For dynamic objects, consider the collision shape together with mass, center of mass, and inertia. Use enough convex pieces to preserve meaningful contacts and set a platform budget for complexity. Test joints and attachments with their actual contact configuration.

**Check** drop, slide, stack, and rotate a representative object on common surfaces. Record penetration, settling time, unwanted rocking, and physics-step cost. Repeat with the simplest viable proxy and justify added complexity by an observed improvement.

### Validate the shipped variant

Keep collision identity and intended geometry stable across visual LODs unless a deliberate physical transition exists. Verify collision data in the cooked build and preserve necessary support when an asset streams or changes state.

**Check** cross each visual LOD boundary while standing on or touching the asset, then stream it out and back in. Compare the cooked collision view with the source fixture and confirm that traversal, queries, and persistent object placement agree.

See [Collision filtering and layers](collision-filtering-and-layers.md) for participation policy and [Runtime collision cooking](runtime-collision-cooking.md) for generated proxies.
