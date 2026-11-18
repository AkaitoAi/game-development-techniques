# Mesh normal and tangent validation

## Problem and design reasoning

A normal map can be correct in the baker and produce seams or rotated highlights in the game when geometry, UVs, or tangent conventions disagree. This guide covers the contract between exported mesh attributes, normal-map baking, and runtime shading. See [Hard-surface game assets](hard-surface-game-assets.md) for broader asset checks.

For *Obduction*, Cyan's Eric Anderson showed a material using differently oriented UV channels that produced incorrect lighting because the shader's tangent basis came from the first channel. Cyan generated an additional tangent basis during import and stored it for the material to reconstruct. The talk explains that the extra work supported reuse of meshes and textures. This documents a particular pipeline solution, while the validation workflow below is an engine-independent recommendation. [Eric Anderson, Building Obduction, GDC 2016, slides 43–51](https://media.gdcvault.com/gdc2016/Presentations/Anderson_Eric_BuildingObduction.pdf)

The MikkTSpace reference contract also explains that the normal-map sampler and shader transformation must agree, including interpolation and handedness. Using its name in two tools is insufficient evidence that all export and shading settings match. [Morten S. Mikkelsen, MikkTSpace reference interface](https://github.com/mmikk/MikkTSpace/blob/master/mikktspace.h)

## Techniques and practical checks

### Record the bake and import contract

Specify normal-map orientation, tangent generation method, UV channel, triangle topology, smoothing boundaries, and which attributes the importer preserves or regenerates. Keep triangulation consistent between the final bake and runtime mesh when a topology change would alter the tangent frame.

**Check** bake and import a fixture with hard edges, UV seams, mirrored islands, and an asymmetric normal detail. Compare under a rotating light after final triangulation and on the packaged target.

### Validate exported attribute quality

Check finite positions, normals, and tangents, valid triangle indices, nonzero triangle area, and usable UV derivatives for surfaces needing tangent-space shading. Treat the bitangent sign as part of the data. Report the asset and affected section when a basis cannot be constructed.

**Check** deliberately introduce a zero-area face, collapsed UV triangle, missing tangent, and nonfinite normal in controlled fixtures. Verify diagnostics identify each issue and the chosen fallback renders predictably.

### Use the basis for the sampled coordinates

Choose how the shader obtains its tangent frame for each normal-map UV set or procedural mapping. Account for mirrored UVs and transform handedness. Under nonuniform scale, transform normals using the appropriate normal transform and reestablish the basis according to the shader's convention.

**Check** rotate a second UV set by 90 degrees, mirror an island, and apply nonuniform and negative scale. Compare the highlight direction with an approved reference and inspect the basis vectors.

### Preserve the contract across generated variants

Repeat checks for each LOD, skinned or deformed path, mesh simplification output, and platform import variant. Evaluate the mesh with a flat normal input first, then its baked detail, so geometry shading and texture effects can be diagnosed separately.

**Check** move through LOD transitions while rotating the light and exercising the deformation range. Record seams, highlight changes, and missing attributes, and verify reimporting the same source reproduces the approved result.
