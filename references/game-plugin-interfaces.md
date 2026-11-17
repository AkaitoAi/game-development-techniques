# Game plugin interfaces

## Problem and design reasoning

Plugins can extend gameplay or authoring tools without coordinated changes to every subsystem. Once a plugin depends on private types, callback timing, or undocumented order, a host update can break existing content. Interface design makes compatibility and lifetime behavior explicit. Internal dependency direction is covered in [Game module dependencies](game-module-dependencies.md), while distribution and load order are covered in [Game mod support](game-mod-support.md).

*Factorio* gives extensions different surfaces for settings, prototype definition, and runtime behavior. Its documented runtime events let plugins act when the game announces a relevant change. The lifecycle provides a useful example of making legal operations depend on the phase of execution. [Wube, Factorio API overview](https://lua-api.factorio.com/latest/index.html) · [Wube, Factorio runtime API](https://lua-api.factorio.com/latest/index-runtime.html)

*Arma 3* exposes an engine-to-extension call interface and a version query that the engine invokes when an extension loads. At GDC 2021, Bohemia Interactive's Karel Mořický described how community extensions depend on other mods and surface gaps in the official extension points. A design implication is that stable contracts and useful failure messages matter to both individual plugin authors and downstream extensions. [Bohemia Interactive, Arma 3 extension interface](https://community.bistudio.com/wiki/callExtension) · [Karel Mořický, The Pandora's Box of Modding in Arma Games, GDC 2021](https://media.gdcvault.com/GDC%2B2021/GDC%2B2021%2B-%2BSlides.pdf)

## Techniques and practical checks

### Define compatibility before activation

Give each plugin a stable identity, supported interface revisions, and required or optional capabilities. Let the host check the contract before registration. Treat binary interface compatibility as its own promise and prefer explicit language-neutral boundary types where the host supports several runtimes.

**Check** load one extension against compatible and incompatible host versions, then disable an optional capability. Confirm each case either initializes with its supported features or reports a useful reason before altering game state.

### Give each callback an execution contract

Specify its thread or scheduler, lifecycle phase, mutation rules, error behavior, and order relative to other hooks. Return registration tokens so the host can track subscriptions and remove them when their owner is disabled.

**Check** register two extensions for the same event, change their discovery order, and disable one while work is pending. Verify the promised order and confirm callbacks cannot run against released resources after teardown.

### Keep state and resources with a clear owner

Pass records or opaque handles across the boundary. State how long returned values are valid, who releases host resources, and what happens on disable or world transition. Version persisted plugin state separately from the host interface.

**Check** open and close several sessions with a representative extension. Record registrations and owned resources across transitions, then reload an older saved plugin state and verify the documented migration or recovery path.

### Define how failures affect game state

Choose which work can fail independently and which failures require a clear session-level stop. Treat simulation-changing hooks differently from optional presentation callbacks. Report the plugin identity and host phase when a hook fails.

**Check** make a local test extension fail while starting, while building optional UI, and while preparing a simulation change. Verify that optional UI can recover and that common gameplay state remains consistent or the session stops with a clear diagnosis.
