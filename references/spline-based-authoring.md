# Spline-based world authoring

## Problem and design reasoning

A road, rail, or river often changes after nearby geometry, collision, and props have been placed. Editing each output separately makes iteration expensive and leaves stale content along the old route. A spline can retain the designer's route and parameters while the tool rebuilds related outputs. Insomniac documented revising its Sunset Overdrive tool when some automatic features gave artists and designers too little control. A useful inference is to expose an override where an individual segment carries unusual gameplay requirements. [David Santiago's GDC 2015 talk](https://media.gdcvault.com/gdc2015/presentations/Santiago_David_Procedural_and_Automation.pdf)

## Shipped examples

- **Sunset Overdrive (2014).** Insomniac's road layout workflow began with curves, swept road surfaces along them, and generated sidewalks and gutters from the remaining block interiors. In the revised system, source-curve control affected corner shape, while road metrics could drive gameplay markup and actor placement. [David Santiago, *Practical Procedural and Automation Techniques for Design, Prototyping and Production of Sunset Overdrive*, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Santiago_David_Procedural_and_Automation.pdf)
- **Call of Duty (terrain technology used since Vanguard, 2021).** High Moon's SIGGRAPH presentation describes road decals projected from above along Bézier curves and composited with terrain layers. The example shows a curve producing material coverage as an output. Use a separate geometry or navigation output when the path needs physical shape or connectivity. [Stéphane Etienne, *Large Scale Terrain Rendering in Call of Duty*, SIGGRAPH 2023, slide 30](https://advances.realtimerendering.com/s2023/Etienne%28ATVI%29-Large%20Scale%20Terrain%20Rendering%20with%20notes%20%28Advances%202023%29.pdf) · [SIGGRAPH course program](https://www.advances.realtimerendering.com/s2023/index.html)

## Techniques and practical checks

### Separate route intent from derived output

Store the source control points, stable segment identifiers, width, elevation rules, and explicitly authored junctions. Generate each needed output from the same input revision, including surfaces or decals, collision, lane markup, and props. Keep designer overrides attached to stable source elements.

**Check** moving one control point, widening one segment, and deleting another. Confirm each output moves with its source, obsolete artifacts disappear, and unrelated overrides survive regeneration.

### Sample by physical distance and geometric error

Use distance along the curve for regular spacing of markings, props, and texture coordinates. Increase geometric sample density where curvature or vertical grade demands it. Use a stable orientation frame and expose explicit banking where the path calls for it.

**Check** a straight segment, a tight bend, an S-curve, and a steep grade. Measure spacing between markers, compare the generated corridor to the source path, and look for frame flips and stretched markings.

### Treat junctions as explicit topology

Represent at-grade connections, level-separated crossings, cul-de-sacs, and transitions with explicit topology. Generate joining geometry and navigation from the same connectivity data. Validate clearance where curves pass over or under one another.

**Check** that a T-junction connects all intended lanes, an overpass has no false connection, and a dead end produces the correct termination. Compare rendered joins with the collision and navigation views.

### Preview locally and validate cooked output

Bound regeneration to changed segments and affected neighbors or tiles. Report self-intersecting ribbons, steep grades, and missing proxies before baking. Use source and configuration revisions to detect stale generated data at build time.

**Check** moving a route across a streaming boundary and rebuilding incrementally. Compare the result to a clean bake, then traverse both directions in the packaged build to inspect continuity, collision, and prop placement.
