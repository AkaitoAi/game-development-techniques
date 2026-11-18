# Runtime spline following

## Problem and design reasoning

An object that advances a spline parameter by a constant amount can change speed as the curve bends or its control points spread apart. Orientation can also flip when a tangent degenerates or a path crosses an awkward up direction. In *Avatar: Frontiers of Pandora*, Massive generated traversal splines whose endpoint tangents align with incoming velocity and the arrival slope, then moved the character along the result. Joel Nilsson explains this in the shipped game's traversal presentation. [Massive, Obstacle Traversal in the Organic World of Pandora, GDC 2024](https://gdcvault.com/play/1034304/Obstacle-Traversal-in-the-Organic) · [Developer slides, spline generation](https://media.gdcvault.com/gdc2024/Slides/GDC%2Bslide%2Bpresentations/GDC_Joel_Nilsson_Obstacle_Traversal.pdf)

Epic's spline API distinguishes distance queries from input-key and time queries. A transferable implementation recommendation is to store gameplay progress in distance units and translate to the curve's parameter using a measured approximation. This is a general technique rather than a claim about Massive's exact parameterization. [Epic spline component reference](https://dev.epicgames.com/documentation/unreal-engine/API/Runtime/Engine/USplineComponent)

## Techniques and practical checks

- Build an arc-length lookup from sampled curve positions, with additional samples where curvature needs them. Advance distance by speed times elapsed simulation time and invert the lookup to evaluate the curve. **Check** a route with straight and sharply curved sections. Compare displacement per second against target speed and tighten sampling until error meets the gameplay tolerance.
- Define a moving frame using the tangent and a stable up or transported orientation. Handle near-zero tangents and intentional banking explicitly. **Check** vertical sections, inflection points, a closed loop, and travel in reverse. Plot basis vectors and inspect unexpected flips or accumulated roll.
- Keep the path as the desired movement source and let the movement owner resolve collision. Define how blocked movement, cancellation, and reaching the end affect remaining progress. **Check** insert an obstacle during travel and interrupt just before completion. Verify that completion fires once and that the object ends at a legal position.
- Version the curve and define the effect of edits or moving parents on progress. Preserve a world-space pose, distance fraction, or authored marker according to the feature's rules. **Check** move the spline parent and change segment length during play. Confirm the chosen continuity policy and verify linked camera or audio followers use the same reference frame.

See [Spline-based authoring](spline-based-authoring.md) for building path content and [Animation root motion](game-animation-root-motion.md) for movement ownership.
