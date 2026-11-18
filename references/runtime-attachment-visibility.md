# Runtime attachment visibility

## Problem and design reasoning

One logical item may have a first-person model, a world model, shadows and attached effects. Hiding its mesh without a representation policy can leave floating effects or hide equipment from observers. The released *Quake III Arena* client uses view-dependent rendering flags for the local player's body and distinguishes local view-weapon presentation from world equipment. Naughty Dog's *Uncharted 2* pipeline describes a shared skeleton across outfits, supporting consistent attachment expectations. The visibility contract below is a portable recommendation. [id Software, Quake III player rendering](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_players.c) · [id Software, Quake III weapon rendering](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_weapons.c) · [Richard Diamant and Judd Simantov, Uncharted 2 Character Pipeline, GDC 2010](https://www.gdcvault.com/play/1012798/Uncharted-2-Character-Pipeline-An)

## Techniques and practical checks

### Define visibility per representation

Specify owner-view, observer-view, reflection, shadow and cinematic behavior for each attachment. Resolve requests centrally so an equip operation cannot overwrite a camera rule.

**Check** Equip an item and switch first person, third person, spectator and a supported reflection view. Verify the intended model and one set of effects.

### Propagate policy to dependent effects

Make lights, particles, decals and loop sounds follow an explicit presentation policy. Avoid assuming mesh invisibility means simulation inactivity.

**Check** Hide an attachment while its muzzle effect or ambient loop is active. Verify the intended dependent behavior and continued gameplay state.

### Publish transitions coherently

Update representation, socket mapping and visibility at a controlled phase. Stamp asynchronous completion with equipment and owner identity.

**Check** Swap equipment during a camera blend and finish the older load last. Confirm no obsolete mesh or effect becomes visible.

### Validate retirement and reuse

Clear temporary overrides when an actor or item is pooled, detached or replaced. Keep gameplay ownership independent of render visibility.

**Check** Hide, drop, pool and reuse an item in sequence. Check render flags, shadows and effects against a fresh instance and ensure delayed callbacks cannot restore old visibility.
