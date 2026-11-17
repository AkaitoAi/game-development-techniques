# Subsurface scattering

## Problem and design reasoning

Skin and some other materials let incident light travel within the surface before it exits nearby. Surface-only diffuse shading can produce sharp light transitions where a face should look softer. Blurring the finished frame softens specular highlights and can smear color across hair, clothing, or the background. Treat the light that spreads through a material as a specific contribution with its own radius and eligibility.

For *Call of Duty: Advanced Warfare* (2014), Sledgehammer's SIGGRAPH 2014 presentation describes a post-processing pipeline built to support the game's photoreal direction. Jorge Jimenez identifies separable subsurface scattering as the skin approach. In a SIGGRAPH 2025 course retrospective, Natalya Tatarchuk also identifies *Destiny* as a shipped use of the screen-space technique and highlights the cost scaling with character coverage. [SIGGRAPH 2014 course archive](https://advances.realtimerendering.com/s2014/index.html) · [Jorge Jimenez, presentation notes](https://www.iryoku.com/blog/) · [Natalya Tatarchuk, SIGGRAPH 2025 retrospective](https://advances.realtimerendering.com/s2025/content/Advances_s2025_Tatarchuk_partI.pdf)

Design inference. A screen-space filter suits expressive close-ups when the renderer can isolate eligible diffuse lighting and supply depth and material boundaries. A smaller correction or an authored simpler response can be appropriate when subjects occupy very few pixels. Choose the approximation with an image and GPU-time comparison on target hardware.

## Implementation and practical checks

### Isolate diffuse lighting

Filter the eligible diffuse contribution in linear color space. Preserve the specular response and composite it with the filtered result. Use a controllable diffusion profile with normalized weights and start with a separable horizontal and vertical filter when compatible with the rendering pipeline. **Check** a face under a moving hard light. Toggle scattering and confirm that the diffuse transition softens while a small specular reflection stays sharp and tracks the light.

### Reject unrelated samples

Gate neighboring samples by depth and material identity. Use normal or surface grouping where steep boundaries require it, then renormalize accepted weights so rejected taps do not darken the surface. **Check** two intersecting profiles and a face next to a saturated background, hair, or an occluding prop. Sweep the camera across the outline and inspect color leaks, halos, and discontinuities.

### Calibrate radius and transmission separately

Author a scattering radius in world or material units and convert to screen size using camera projection and depth. Treat thin-region transmission as an optional separate response driven by thickness and light direction. **Check** the same material at two camera distances and fields of view, then backlight a thin and thick region. Confirm that the apparent world-space softness stays consistent and that transmission is strongest in the intended region.

### Define quality tiers against a coverage budget

Measure filter passes and intermediate-buffer traffic at close-up, normal gameplay, and crowd scale. Compare lower tap counts, reduced resolution, or a simpler material response with the reference. **Check** several faces at maximum expected screen coverage and a rapid shot transition. Verify GPU time, temporal stability, and fallback appearance across supported quality tiers.
