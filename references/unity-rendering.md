# Unity rendering, shaders, and visual effects

## Problem and design reasoning

A pass can look correct in an isolated preview while running at the wrong point in a frame, failing at another quality tier, or overwhelming the GPU in a crowded scene. Unity's URP guide uses a Render Objects feature to draw a character silhouette behind obstacles. The renderer can present occlusion feedback without changing camera behavior or level layout. [U2, PDF pp. 76–78](unity-books.md#u2)

Inspect the project's Unity, render-pipeline, Shader Graph, and VFX Graph package versions before mapping book examples to the current implementation. The URP guide covers 2021 LTS and the Jettelly book includes examples using legacy shader syntax. Apply the underlying render-state and shader principles using the project's supported APIs.

## Techniques and practical checks

- **Specify a custom pass contract.** Identify camera type, object filter, injection point, depth test, blending, and required buffers. **Check** a visible actor, an occluded actor, a transparent obstacle, and a secondary camera. Step through the frame to verify draw order and depth behavior. [U2, PDF pp. 76–78, 120–123](unity-books.md#u2)
- **Budget keyword combinations.** Keep independent material variants only when their combinations are useful. Verify the actual player-build variants rather than relying on an Editor preview. **Check** representative materials and a runtime keyword change in a build. Watch for fallback shaders and measure compilation or warmup hitches separately from pass cost. [U5, PDF pp. 52–53](unity-books.md#u5)
- **Reuse effect logic in VFX Graph.** Extract a shared calculation when multiple effects need the same behavior and expose parameters explicitly. **Check** two consuming effects with distinct inputs. Change the shared graph and verify that both update while keeping their intended appearance. [U6, PDF pp. 27–28](unity-books.md#u6)
- **Budget transparent coverage.** Reduce particle quad coverage, mesh complexity, or emission where decorative effects overlap heavily. **Check** several effects over the largest expected screen area. Inspect overdraw and GPU time while checking that essential gameplay cues remain readable. [U6, PDF pp. 99–102](unity-books.md#u6)
