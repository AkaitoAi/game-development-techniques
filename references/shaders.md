# Shaders

## Problem and design reasoning

**Problem.** Material detail vanishes at some distances, moving geometry disagrees across passes, or a costly effect dominates only on certain surfaces. Capture the failing geometry and identify which inputs and passes are involved.

**Design reasoning.** Valve's baked lighting and normal-map combination and Guerrilla's deferred foliage shading are documented examples. An inferred priority is to buy visible detail at the stage where it fits the quality and performance budget. See the shipped examples below for sources.

Use this reference for material shading, deformation, image effects, and GPU shader cost. For scene illumination and shadows, see [Lighting](lighting.md). For whole-frame bottlenecks, see [Performance](performance.md).

## Shipped examples

**Half-Life 2 (2004).** Valve's radiosity normal mapping stored precomputed illumination in a directional basis. At runtime, a detailed surface normal from a texture weights the baked lighting contributions so small surface features respond to the direction of incoming light. The technique combines relatively coarse baked lighting with finer material detail. [Valve developers, SIGGRAPH 2006 course notes](https://steamcdn-a.akamaihd.net/apps/valve/2006/SIGGRAPH06_Course_ShadingInValvesSourceEngine.pdf)

**Horizon Forbidden West (2022).** Guerrilla's foliage and alpha-tested geometry first writes a visibility buffer. Subsequent compute passes analyze and shade the visible samples. The compute design also supports software variable-rate shading, letting the renderer vary shading work across the image. Guerrilla reports overlapping parts of this work with other rendering on PlayStation hardware. [Guerrilla developer presentation](https://www.guerrilla-games.com/read/adventures-with-deferred-texturing-in-horizon-forbidden-west)

**What changed.** Valve reduced runtime lighting work with precomputed data and recovered surface detail in the shader. Guerrilla reorganized when material shading happens to focus work on visible foliage. Both approaches use a cheaper representation before evaluating finer detail. Directional baking remains useful for stationary illumination. A separate visibility pass is worth evaluating when expensive shading and overdraw justify its extra passes and bandwidth.

## Techniques and practical checks

### Define the shader's data contract

Specify each texture channel, sampler and coordinate space before implementation. Decode color textures to linear space for lighting and use linear sampling for data textures such as roughness and normals. Reconstruct a tangent-space normal using the mesh's tangent basis and handedness, then normalize the result. Keep the color transform under the render pipeline's control so it is applied consistently.

**Check** on a sphere, a mirrored UV seam, and a nonuniformly scaled mesh. Rotate a single light and inspect debug outputs for the normals and packed data channels. Compare a neutral color patch against a reference under fixed exposure to detect incorrect gamma handling.

### Match material features to viewing distance

Expose a small set of material parameters and combine shared features with masks. Bake stable fine detail into textures where it reads at gameplay distance. Use mipmaps and appropriate filtering for moving views. For fine specular detail, measure whether roughness filtering or fading the detail normal with distance reduces shimmer. Choose quality tiers based on features that make a visible difference at the target resolution.

**Check** from close, medium, and distant views while panning under both broad and sharp highlights. Compare the tiers with the same exposure and lighting. Look for shimmer, visible transitions between tiers, texture memory changes, and GPU time.

### Keep deformation and coverage consistent across passes

When a vertex shader moves geometry, share the deformation inputs and time between color, depth, shadow, and motion-vector passes as applicable. Expand culling bounds to cover the supported deformation. For cutout materials, align the coverage rules across passes and use mip handling that preserves the intended silhouette. If partial transparency is needed, evaluate blend order and overdraw under representative layering.

**Check** with animated foliage at grazing angles and against a bright background. Compare depth, shadow, and motion-vector debug views with the visible silhouette. Sweep the camera past the culling boundary and verify that moving geometry stays visible and temporal effects remain stable.

### Measure GPU cost and shader compilation separately

Profile the shader in a representative scene and isolate changes to arithmetic, texture reads, screen coverage, or overdraw. Compare a uniform runtime branch with a compiled variant where supported. Reserve variants for combinations that justify the extra build and cache costs. Warm or prebuild common shader and pipeline states if the target renderer supports it.

**Check** on target hardware with flat opaque surfaces and a dense overdraw case. Record GPU pass time for both. Then run a first traversal with caches cold and a second with caches warm, logging shader or pipeline creation events alongside frame times. Confirm that a change improves the intended bottleneck and preserves visual output within the project's tolerance.
