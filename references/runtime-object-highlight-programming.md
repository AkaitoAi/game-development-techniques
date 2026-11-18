# Runtime object highlight programming

## Problem and design reasoning

A hover, objective marker, damage cue, and teammate outline can all request a change to the same object's presentation. A boolean toggle loses track of which request is still active, while changing a shared material can affect unrelated objects. Valve's released Source SDK includes combat-character glow state, explicit glow creation and destruction, and client-side enablement. This is evidence for a lifecycle owned by the renderable character; it does not establish the priority policy proposed below. [Valve, Source SDK combat-character glow implementation](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/client/c_basecombatcharacter.cpp)

Michal Valient's GDC 2009 presentation on shipped *Killzone 2* documents separate rendering stages and an extensive post-processing stage. The transferable implementation inference is to make highlight rendering fit the actual pipeline and depth contract, then keep the requesting gameplay state independent from that rendering technique. The talk is a renderer case study, rather than evidence that Killzone used the highlight protocol below. [Guerrilla, The Rendering Technology of Killzone 2, GDC 2009](https://www.guerrilla-games.com/read/the-rendering-technology-of-killzone-2)

## Techniques and practical checks

### Track requests by owner

Represent a request with the target identity, target generation, request owner, reason, priority, and optional expiry. Resolve active requests into a single presentation state. Release one owner's request independently of the others and define how equal priorities combine.

**Check** hover an objective object, add a damage cue, then remove the hover. Confirm the remaining requests still display according to priority. Destroy and reuse the target while an expiry is queued and confirm the replacement receives no old highlight.

### Keep presentation state local

Use the engine's per-instance parameters, outline registration, or dedicated draw path where supported. Keep the authored material intact. Record any temporary material or renderer ownership so teardown restores the correct state.

**Check** highlight one of two objects sharing the same material, then disable and re-enable it. Verify only the intended object changes and that repeated activation does not increase the count of material instances or outline registrations.

### Specify the visibility contract

Choose whether the cue appears on visible surfaces, behind occluders, or both. Define behavior for transparent objects, attachments, multiple cameras, and reduced-effects settings. Use another cue when color alone cannot communicate the state.

**Check** view an object through an opaque wall and transparent pane with two supported camera modes. Change quality settings and remove color information from the capture. Verify the result follows the intended visibility policy and the reason for the cue remains recognizable.

### Publish coherent updates

Send the resolved highlight state with the same render-instance revision as its target. Batch updates at a defined phase, retire registrations before reusing an instance, and bound the visible-highlight budget.

**Check** delay a highlight update while unloading its scene and reusing the render slot. Confirm the stale update is rejected. Profile the largest supported highlighted population and record draw cost, registration count, and update time.

See [Runtime material parameter management](runtime-material-parameter-management.md) for parameter scope and [Runtime render instance management](runtime-render-instance-management.md) for render identity.
