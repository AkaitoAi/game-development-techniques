# Runtime surface classification

## Problem and design reasoning

Audio, decals, particles, and contact behavior can disagree when each subsystem guesses a surface from a renderer name or its own lookup. The query may hit a simplified collider, a submesh, terrain, or a temporary replacement, each requiring a defined classification path. In the released *Quake III Arena* movement code, the ground trace's surface flags select ordinary, metal, or suppressed footstep events. Its impact presentation also consults surface flags. That demonstrates a shared authored signal crossing gameplay and presentation. [id Software, ground-surface selection](https://github.com/id-Software/Quake-III-Arena/blob/master/code/game/bg_pmove.c) · [id Software, impact-surface selection](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_weapons.c)

Stephen Hodde's GDC talk on *Red Faction: Armageddon* describes the system and content challenges of physics-driven destruction sound. It motivates consistent contact metadata, but does not establish a specific surface registry. Epic documents physical-material surface types as inputs for sounds and decals. The resolution contract below is an engine-independent recommendation. [Hodde, GDC 2012](https://gdcvault.com/play/1015604/contactUs) · [Epic Games, Physical Materials Reference](https://dev.epicgames.com/documentation/en-us/unreal-engine/physical-materials-reference-for-unreal-engine)

## Techniques and practical checks

### Define one classification result

Return a stable semantic surface ID, source revision, and a defined unknown/default result. Specify precedence among an explicit gameplay override, collision metadata, submesh or terrain information, and the fallback. Let sound, effects, and physics map that shared ID into their own response tables.

**Check** query one object through each supported collision mode and verify all consumers receive the intended ID. Include a multi-material mesh, missing metadata, and an intentionally overridden surface.

### Preserve the hit context

Resolve classification from the actual hit's collider, shape, face, or terrain sample where supported. State which information simple collision retains. Avoid silently using a visual submesh index that has no corresponding collision mapping.

**Check** hit each side of a multi-surface object using a ray, shape sweep, and actual contact. Compare the returned classifications with an authored fixture and record any deliberate simplification.

### Version changes and delayed results

Give a changed surface mapping an explicit revision. Validate delayed query results against the object identity and mapping revision before using them. Decide whether an existing scrape follows a material change immediately or at the next contact onset.

**Check** delay a query, replace the object or surface mapping, and deliver the old result. Verify it cannot select a response for the replacement. Change a contacted surface during a sustained loop and confirm the chosen transition policy.

### Validate assets and fallbacks together

Check that each reachable surface ID has the required response or an intentional fallback in every consumer. Trace the chosen ID and lookup source in a debug view. Keep semantically different responses from depending on texture filenames.

**Check** load a representative level and enumerate actual hit results across props and terrain. Introduce one unmapped ID in a test asset and verify a usable fallback plus a diagnostic identifying the owner and missing response.

See [Runtime footstep programming](runtime-footstep-programming.md), [Impact sound design](impact-sound-design.md), and [Collision proxy authoring](collision-proxy-authoring.md).
