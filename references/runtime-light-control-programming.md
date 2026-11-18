# Runtime light control programming

## Problem and design reasoning

Gameplay, cinematics, weather, and damage can all attempt to change the same light. Uncoordinated writes cause flicker, lost transitions, and lights restored to obsolete values. Runtime control needs an authoritative state and explicit override rules.

In *Quake III Arena*, client entity effects unpack constant-light color and intensity from entity state and submit the result at the interpolated entity origin. This is a compact documented example of gameplay state driving render submission. Modern lighting adds more dependent work. Michal Valient's GDC 2014 *Taking Killzone Shadow Fall Image Quality into the Next Generation* discusses indirect lighting and shadow optimizations, with temporal coherence as a major theme. Neither source documents the arbitration system recommended below. [id Software, CG_EntityEffects](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_ents.c) · [Michal Valient, GDC 2014](https://www.gdcvault.com/play/1020770/Taking-Killzone-Shadow-Fall-Image)

4A Games describes refreshing indirect-light probes across frames in *Metro Exodus PC Enhanced Edition*. The practical implication is that changing a direct light can also require observing delayed indirect response. This is an inference from the documented rendering system. [4A Games, technical dive](https://www.4a-games.com.mt/4a-dna/in-depth-technical-dive-into-metro-exodus-pc-enhanced-edition)

## Techniques and practical checks

### Resolve requests before writing renderer state

Keep authored defaults, gameplay state, and temporary overrides separate. Resolve enabled state, color, intensity, range, and shadow policy once per update. Give competing requests documented priority and blend rules. Use ownership tokens so cancellation removes only the request that created the token.

**Check** overlap weather, damage flicker, and a cinematic override. Cancel each in different orders and verify the surviving state is deterministic.

### Define timing and units

Use explicit intensity units and color representation supported by the target renderer. Specify the clock for fades, including pause behavior. Evaluate transitions from their start and end values rather than repeatedly modifying the last frame's output.

**Check** compare transitions at low and high frame rates, pause halfway, and reverse direction. Verify duration and final values remain consistent.

### Connect dependent representations

Drive visible bulb emission and the actual light from the same logical state with separate calibrated mappings. Notify shadow or indirect-light caches when required by the engine. Do not assume emissive appearance automatically provides illumination.

**Check** switch a lamp off beside a wall and moving character. Inspect bulb appearance, direct light, shadow, and indirect decay under fixed exposure.

### Bound live-light cost and recovery

Apply quality policies after gameplay resolution, preserving important signals while reducing secondary shadow or range costs. Restore state through stable light identifiers after load or streaming rather than stale renderer handles.

**Check** trigger the largest simultaneous lighting event, unload and reload its area, and restore a save mid-transition. Record GPU cost and verify the intended final state.

See [Lighting](lighting.md) for rendering approaches and [Runtime material parameter management](runtime-material-parameter-management.md) for material mutation.
