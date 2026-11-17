# Game module dependencies

## Problem and design reasoning

A change to a low-level math or gameplay header can trigger a rebuild of editor, client, and server targets. This often points to broad public interfaces or dependencies leaking through them. Trace source imports, links, and runtime service lookups separately. A clean compile graph can still hide tight runtime coupling.

**Destiny.** Bungie wanted code layers that supported engine sharing and fast rebuilds. In his GDC 2015 retrospective, Chris Butcher reported that the boundaries became messier than planned and compile times were costly. He identified splitting layers into libraries earlier as an opportunity. The practical inference is to measure rebuild fan-out and enforce boundaries as the codebase grows. [Chris Butcher, Lessons from the Core Engine Architecture of Destiny, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Butcher_DestinyEngine_GDC2015_final.pdf)

**Quake III Arena.** The released source builds server gameplay as its own module and defines the client game’s imports and exports in a public contract. This is an example of isolating game policy from engine services through an explicit interface. It does not imply every feature needs dynamic loading. [Server game build rules](https://github.com/id-Software/Quake-III-Arena/blob/master/code/game/Conscript) · [Client game contract](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_public.h)

## Techniques and practical checks

- **Make ownership visible in the dependency graph.** Put reusable runtime services below game policy. Let a composition layer connect concrete services and feature modules. Record allowed dependency directions in the build. **Check** generate an import or project-reference graph and identify cycles. Build a runtime-only target and confirm that no editor-only modules or symbols are included.
- **Narrow the interface where a dependency crosses a boundary.** Expose the operation a caller needs and specify the lifetime of inputs and outputs. For example, a targeting feature can request a world query through a small interface instead of importing the renderer and physics implementations together. **Check** build the caller against a stand-in and verify an invalid or expired query result is handled through the same public contract.
- **Measure rebuild costs before splitting targets.** Compare edits to a private implementation with edits to a public interface. If the latter invalidates many targets, inspect public imports and re-exports before moving files between libraries. **Check** repeat representative private and public edits, recording rebuilt files, link time, and elapsed time. Compare with the original graph.
- **Validate optional modules as real configurations.** Declare required and optional dependencies and define how an absent capability is reported. Version contracts when independently built modules must coexist. **Check** run with a required module missing, an optional one absent, and an incompatible interface version. Verify the required or incompatible cases are reported before the first gameplay call and the supported optional case can still play.
