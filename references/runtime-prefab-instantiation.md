# Runtime prefab instantiation

## Problem and design reasoning

An authored object template can contain several components and references that must agree before gameplay sees the instance. Publishing a root object while its children or dependencies are incomplete can produce intermittent startup failures. In Assassin's Creed Unity, a gameplay system issues a spawn request and obtains a token before spawning the authoritative entity, while peers create replicas. That shipped design establishes an explicit creation decision. Bungie's Destiny talk discusses validating when data creation and destruction are safe in a job-based frame. The staged instantiation contract below is a portable inference from these concerns. [Charles Lefebvre, Networking Gameplay and AI in Assassin's Creed Unity, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Lefebvre_Charles_NetworkingGameplayAndAI.pdf) · [Barry Genova, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-)

## Techniques and practical checks

### Plan the instance before activating it

Resolve the template version, required assets, spawn transform, ownership, and initial component values. Construct a staged instance, connect its internal references, and publish it at the frame phase that owns world mutation. Specify whether children inherit authority, scene ownership, and cancellation from the root.

**Check** instantiate a template with a missing dependency and another with a malformed child reference. Verify a failure leaves no active partial object and the valid template initializes identically across repeated runs.

### Separate construction from gameplay callbacks

Give components a way to populate references before registration, collision activation, subscriptions, or ordinary ticks begin. Publish the instance only after its required components report readiness. When components require ordered startup, express the dependency and detect cycles.

**Check** randomize component construction order and spawn during an encounter callback. Verify the first tick observes a complete instance and initialization executes once.

### Give each request a distinct identity

Associate a spawn request with an owner generation and an instance ID. Validate both before completing asynchronous construction. When multiple machines or systems can request one logical object, assign one creation authority and make duplicate requests resolve to the same outcome.

**Check** cancel a request, reuse its requester, and complete the old request afterward. Also submit a duplicate request. Verify there is one intended object and no mutation of the replacement owner.

### Budget integration separately from loading

Measure allocation, component setup, collision registration, and render integration individually. Limit per-frame integration where gameplay permits and expose a readiness state while waiting. On failure or scene unload, dispose of staged children and release acquired assets.

**Check** spawn a representative burst while traversing a streaming boundary. Record integration time, pending request age, and active object count. Unload the scene halfway through and verify all staged instances reach a terminal result.

For reusable active objects, see [Game object pooling](game-object-pooling.md). For dependency leases, see [Asynchronous asset lifetimes](async-asset-lifetimes.md).
