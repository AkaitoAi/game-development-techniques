# Real-time ray tracing

## Problem and design reasoning

A reflection computed from the visible image loses objects when they leave the camera view or hide behind closer surfaces. A ray query against a scene representation can recover those hits, at the cost of acceleration-structure maintenance, traversal, and shading. Sparse rays introduce noise and make reconstruction a separate engineering problem. DICE identified off-screen visibility as a limitation of screen-space reflections in *Battlefield V*. For another project, infer whether that limitation matters by capturing the specific shot or surface that fails. [DICE and EA, Battlefield V real-time ray tracing](https://www.ea.com/amp/news/battlefield-5-real-time-ray-tracing)

## Shipped examples

- *Battlefield V* shipped ray-traced reflections. DICE's GDC 2019 presentation, **It Just Works: Ray-Traced Reflections in 'Battlefield V'**, covers shader generation through denoising. The scope isolates reflection quality while the broader renderer remains hybrid. [DICE, GDC 2019](https://www.gdcvault.com/play/1026282/) · [EA SEED, I3D 2021](https://media.contentapi.ea.com/content/dam/ea/seed/presentations/seed-i3d21-trace-all-the-rays.pdf)
- *Cyberpunk 2077* released Overdrive as a PC technology preview in patch 1.62. CD Projekt Red and NVIDIA presented the path-traced mode at GDC 2023. This offers a wider lighting scope than a single ray-traced effect, with a correspondingly different hardware and performance target. [CD Projekt Red, patch 1.62](https://www.cyberpunk.net/en/news/47875/patch-1-62-ray-tracing-overdrive-mode) · [NVIDIA, GDC 2023 presentation coverage](https://blogs.nvidia.com/blog/cyberpunk-2077-path-traced-visuals-gdc/)

## Techniques and practical checks

1. **Choose the visibility problem first.** Isolate one reflection, shadow, or indirect-light scenario. Define which surfaces can use cheaper screen-space or probe data and when a ray must query the scene. Keep an explicit fallback for missed rays and unsupported devices.  
   **Check** by moving a bright object from on-screen to off-screen beside a glossy surface. Compare the ray path, fallback transition, and frame-time delta under the same camera sweep.
2. **Define the ray scene.** Decide which geometry, materials, and instance transforms participate. Track the cost of adding skinned, deforming, or fast-moving assets. Rebuild or update structures according to the backend's correctness and performance requirements.  
   **Check** moving, destroying, and respawning a reflected object. Verify that its appearance in ray results updates in step with visible geometry and watch the structure-update cost during a dense scene transition.
3. **Test temporal reconstruction under motion.** Use trustworthy motion and surface data to identify valid history. Reject stale samples at disocclusions and camera cuts. Preserve edges when filtering noisy samples.  
   **Check** with a fast pan, abrupt cut, moving emissive, and thin geometry. Inspect raw rays, motion vectors, rejection masks, and final output for trails, flicker, or light leaking across boundaries.
4. **Budget the entire effect.** Measure structure upkeep, tracing, shading, reconstruction, and memory separately. Tune ray count, render resolution, roughness thresholds, or effect coverage to fit the target hardware and workload.  
   **Check** in a reflective indoor scene and a geometry-heavy exterior. Compare the same camera path across quality tiers and record GPU frame-time percentiles and memory use alongside image quality.
