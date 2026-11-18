# Runtime decal programming

## Problem and design reasoning

A hit can request a surface mark after its receiver moves, unloads, or returns to a pool. Repeated impacts can exhaust storage or evict only part of a mark that spans several polygons. The runtime needs a placement record and retirement contract as well as a rendering technique.

The released *Quake III Arena* implementation builds an oriented basis from the impact normal, clips a projected quad into fragments, and stores persistent fragments in a bounded pool. When the pool fills, it retires the oldest timestamp group. Temporary marks go directly to the renderer, while persistent marks receive age-based fading. This provides concrete shipped evidence for bounded mark storage and distinct transient and retained paths. [id Software, Quake III Arena mark implementation](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_marks.c)

Michal Valient's *Killzone 2* talk shows decals modifying material data before lighting. That establishes where the result belongs in its renderer, while the placement, receiver-generation, and request protocols below are portable recommendations. They extend the rendering contract to moving and replaceable gameplay objects. [Guerrilla, Deferred Rendering in Killzone 2, Develop Brighton 2007](https://www.guerrilla-games.com/media/News/Files/Develop07_Valient_DeferredRenderingInKillzone2.pdf)

## Techniques and practical checks

### Capture a placement request with an explicit space

Record the surface point, normal, material selection, size, and orientation seed when the interaction resolves. Decide whether the mark remains in world space or is attached to a receiver-local frame. Include receiver identity and generation when attachment requires the receiver to remain valid. Transform normals correctly under nonuniform scale and reject a degenerate basis.

**Check** place identical marks on a static wall, translating door, and rotated object with nonuniform scale. Move each receiver before consuming a delayed request. Verify the chosen attachment policy and surface orientation against a drawn local basis.

### Publish complete marks

Treat a projected mark's fragments as one logical allocation. Build and validate geometry before publishing the visible record. If projection runs asynchronously, validate the receiver and geometry revision again at publication. Define a fallback when clipping produces no acceptable surface.

**Check** generate a mark crossing a wall corner, delay publication, then replace or destroy the receiver. Confirm the obsolete request produces no visible fragments and any reserved memory returns to its baseline.

### Give removal one owner

Track expiry, explicit removal, receiver retirement, and region unload through the same logical mark identity. A mark can fade while retaining its allocation until the final visible frame or renderer retirement boundary. Keep authoritative gameplay state independent from whether a cosmetic mark survives a quality-tier eviction.

**Check** expire a mark while unloading its region, then deliver a second removal notification and reuse the slot. Verify one retirement, no fading applied to the replacement, and the specified persistence behavior after reload.

### Budget requests and retained results

Limit incoming placement work, active mark geometry, and overlap cost separately. Evict whole logical marks using an explicit policy when allocation fills. Record the oldest pending request and dropped decorative work so a brief burst cannot become an unbounded queue.

**Check** exceed the placement and retained-mark limits with repeated impacts across several receivers. Verify deterministic whole-mark eviction, bounded queue length and allocation, and acceptable placement latency in the representative scene.

See [Decal rendering](decal-rendering.md) for receiver filtering and pass behavior, and [Game object generation counters](game-object-generation-counters.md) for stale receiver references.
