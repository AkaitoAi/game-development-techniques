# Mobile shader budgets

## Problem and design reasoning

A shader that performs well on one phone can saturate texture bandwidth, shader execution, or render-target traffic on another. Define per-pass budgets in milliseconds for actual scene coverage, overdraw, and lighting density on your supported device tiers. Keep the material model and the work required to produce it visible to artists. Android’s shader guidance identifies sampling, lighting, and transparency as cost drivers. [Android materials and shaders guide](https://developer.android.com/games/optimize/materials?hl=en)

**Shipped examples.** In the GDC 2014 talk *Scaling from Mobile to High-End PCs: The Tech of Broken Age*, Double Fine described compiling permutations to remove features on weaker GPUs and handling a separate alpha sample on its Android ETC1 path. For *Diablo Immortal*, NetEase engineer Hao Wu explained at GDC 2023 that the art team needed dynamic lighting and shadows in outdoor areas, so the team reworked its pipeline to accommodate mobile performance, memory, and compatibility constraints. **Inference.** An expensive feature earns a budget when it serves a specific visual goal. Preserve that goal and pick how to pay for it on each device class. [Double Fine GDC 2014 slides](https://www.p1xelcoder.com/resources/GDC_2014_Scaling_from_Mobile_to_High-End_PCs_The_Tech_of_Broken_Age.pdf) · [Hao Wu’s GDC 2023 session](https://www.gdcvault.com/play/1029299/Rendering-Lights-and-Shadows-on)

### Locate the cost on the target GPU

**Check** a representative scene on a low-tier and upper-tier phone. Fix render resolution, viewing path, and quality settings. Compare a baseline with one change at a time: substitute a cheap shader, reduce overlapping transparent layers, or remove a render-target dependency. Compare pass timings and available hardware counters to identify which change relieved the bottleneck. [Android materials and shaders guide](https://developer.android.com/games/optimize/materials?hl=en)

### Stress materials where they fill the screen

**Check** the closest plausible camera view, several overlapping effects, and the highest number of relevant lights. Compare these results with the standard camera path. Record fragment work and render-target traffic where your profiler exposes them. A material that meets budget on a small prop may exceed it as a full-screen effect. [Hao Wu’s GDC 2023 session](https://www.gdcvault.com/play/1029299/Rendering-Lights-and-Shadows-on)

### Bound permutations and inspect the shipped fallback

**Check** the generated shader for each step down in your material quality policy. Confirm disabled features remove their dependent samples and operations, and check that all selected variants exist in a clean release build. Use image comparisons to verify readable outlines, transparent edges, and important foreground details on the fallback. [Double Fine GDC 2014 slides](https://www.p1xelcoder.com/resources/GDC_2014_Scaling_from_Mobile_to_High-End_PCs_The_Tech_of_Broken_Age.pdf)

### Reduce precision only with a visual threshold

**Check** reduced precision on multiple physical GPU families, especially camera motion, long distances, smooth gradients, and animation-driven parameters. Use full precision for values that fail, then remeasure GPU time and check the compiled result where tools allow it. Keep the winning change only if the target devices gain enough performance to justify the visual trade-off. [Android materials and shaders guide](https://developer.android.com/games/optimize/materials?hl=en)
