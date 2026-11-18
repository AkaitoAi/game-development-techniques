# Runtime scene transition programming

## Problem and design reasoning

A destination can finish reading from disk before collision, spawn placement, scripts, and input routing are ready. Meanwhile, an old load request may complete after the player has selected another destination. A transition coordinator needs to distinguish preparation from activation and protect both worlds' lifetimes.

Elan Ruskin's GDC 2015 postmortem describes how Insomniac changed technology built around level-based games and airlock loading to support Sunset Overdrive's fast open-world traversal. The case illustrates why loading and instantiation assumptions must fit actual movement and content scale. [Insomniac, Streaming in Sunset Overdrive's Open World, GDC 2015](https://www.gdcvault.com/play/1022268/contactUs)

Valve's shipped Half-Life SDK explicitly constructs transition connections and records landmarks for movement across level boundaries. The coordinator recipe below generalizes these examples into a request-owned state machine. Its revision and rollback rules are recommended implementation techniques rather than claims about either game's internal coordinator. [Valve, Half-Life change-level implementation](https://github.com/ValveSoftware/halflife/blob/master/dlls/triggers.cpp)

## Techniques and practical checks

- **Use one transition owner.** Represent request, prepare, activate, handoff, and retire as explicit states. Give each request a generation and target identity. Async completion must validate both before advancing the state. **Check** request A, then request B before A finishes, and return A's completion last. Verify B receives one handoff and A releases its staged resources.
- **Stage destination readiness.** Track required content, collision, navigation or scripts where needed, a valid spawn, and a presentable first camera. Define a fallback for each required failure. Keep readiness independent from an approximate download progress percentage. **Check** delay collision and fail a required dependency after visual assets are ready. Confirm input is held in a usable loading state and the transition can recover.
- **Commit persistent actors deliberately.** Map position and orientation using boundary anchors where appropriate. Move or reconstruct persistent actors once, rebind services, and establish which scene owns their resources. Retire the source only after the target and persistent objects are valid. **Check** cross with an attached prop, active ability, and persistent companion. Verify each object has one owner and no scene-local reference points into retired content.
- **Trace cancellation and retirement.** Record the transition ID, state changes, resource leases, and handoff frame. Cancel pending work through its owner and wait for callbacks that can still reference old state before retirement. **Check** repeat forward and backward transitions, cancel near activation, and destroy the requestor. Compare memory after repeated cycles and verify camera, audio, input, and UI each attach to the current scene.

For player-facing continuity, read [Loading and transitions](loading-and-transitions.md). For lower-level dependency leases, read [Async asset lifetimes](async-asset-lifetimes.md).
