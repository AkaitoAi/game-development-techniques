# Runtime effect parameter programming

## Problem and design reasoning

A visual effect can receive charge, velocity, target position, damage type, and quality changes from several systems. Without ownership and lifetime rules, update order determines appearance and pooled instances inherit stale values. Brandon Whitley's SIGGRAPH 2017 talk *The Destiny Particle Architecture* describes *Destiny 2* particle nodes whose size and color parameters are expressions, supported by an HLSL converter and CPU/GPU bytecode execution. Bungie's stated goals include fast iteration, expressive effects, and performance. [SIGGRAPH course and talk abstract](https://advances.realtimerendering.com/s2017/index.html)

An adapter must also respect the target runtime's override behavior. Unity's documented VisualEffect setter enables a property override, while ResetOverride restores the asset default. This illustrates why writing a default-looking value and releasing an override can have different meanings. The ownership and lifecycle protocol below is an engine-agnostic recommendation, not a claim about Destiny's gameplay code. [Unity SetFloat](https://docs.unity3d.com/cn/6000.0/ScriptReference/VFX.VisualEffect.SetFloat.html) · [Unity ResetOverride](https://docs.unity3d.com/cn/current/ScriptReference/VFX.VisualEffect.ResetOverride.html)

## Techniques and practical checks

### Define a typed parameter contract

Record semantic name, type, units, coordinate space, range, default, and sampling policy. State whether a value is captured at spawn or updated continuously. Resolve runtime property identifiers through an adapter, and diagnose missing or incompatible fields during content validation.

**Check** bind an effect with a renamed property, wrong type, and missing target. Confirm validation identifies the effect and field, and playback uses the documented fallback without invalid values reaching simulation.

### Give every final value one owner

Collect gameplay, cinematic, accessibility, and quality inputs through an explicit resolver. Choose priority, addition, multiplication, or another defined composition per field. Commit one resolved snapshot at the intended simulation boundary. Keep cosmetic outputs from becoming authoritative gameplay state.

**Check** apply a charge change and cinematic override in reversed call orders. Confirm both produce the same resolved values and that removing the override restores the current gameplay value.

### Initialize and release overrides with instance lifetime

Initialize all required parameters before activation. Associate deferred writes with an instance generation or equivalent lifetime token. On release, unsubscribe writers, clear targets, and reset overrides according to the adapter's semantics before returning an instance to its pool.

**Check** reuse one instance for different actors and effect types while delaying an old callback. Confirm the old callback cannot alter the new instance and no previous target, charge, or color leaks into it.

### Make update costs and suspension visible

Batch writes, avoid unchanged updates where useful, and define behavior when an effect is culled, paused, detached, or resumed. Specify whether resumption samples current state or replays historical changes. Include CPU/GPU transfer and simulation costs in measurement.

**Check** suspend a charging effect, move its target, change quality, then resume it. Verify documented appearance and timing, finite parameter values, and bounded update cost under maximum expected effect concurrency.
