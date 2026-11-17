# 2D rendering and pixel art

## Problem and design reasoning

**Problem.** Sprites shimmer during subpixel camera motion, inherit the wrong draw order, or change alignment after atlas repacking.

**Design reasoning.** Celeste's rendering write-up connects a chosen low-resolution presentation with a practical lighting cost. A design inference is to treat pixel scale and sprite placement as constraints on the whole renderer, then allocate detail where it survives the final output.

## Shipped examples

**Doom (1993).** The released Linux renderer projects world sprites into screen space, orders them by scale, and clips their columns against intervening wall segments. This is a useful historical example of explicit sprite visibility in a world with depth. [id Software sprite renderer](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/r_things.c)

**Celeste (2018).** Developer Noel Berry explains how the game's 320×180 presentation helped make low-resolution light masks practical. The implementation packs lights into regions and channels of a shared texture, then composites them in a separate pass. The chosen image resolution influenced both the look and the cost of an effect. [Noel Berry's rendering breakdown](https://noelfb.com/posts/celeste_lighting/) · [ACMI developer conversation, Melbourne International Games Week 2025](https://www.acmi.net.au/whats-on/migw-2025/celeste-in-conversation/)

**Cuphead (2017).** Studio MDHR's developer session describes the team's traditional animation process. The art pipeline used hand-drawn animation and watercolor backgrounds, an example of a 2D game whose visual identity depends on the source art surviving composition and scale changes. [Studio MDHR, GDC 2017 Animation Bootcamp](https://gdcvault.com/play/1024315/Animation-Bootcamp-Cuphead-Process-and) · [Nintendo developer interview](https://www.nintendo.com/jp/topics/article/17ac91b9-64be-11e9-a16a-063b7ac45a6d)

## Techniques and practical checks

### Set a pixel sampling policy

For pixel art, pick an internal resolution and a world-to-pixel scale. Keep movement and camera smoothing in continuous coordinates, then quantize the rendered view and relevant sprite placement to one consistent pixel grid. Present at integer scale with nearest sampling where uniform pixels are a requirement. Decide explicitly how to handle window sizes between integer multiples. For high-resolution painted art, evaluate filtered sampling and asset resolution separately.

**Check** a one-pixel checkerboard, slow diagonal movement, a panning camera, and a moving platform carrying the player. Inspect frames at native, doubled, and odd-size output resolutions for uneven pixels, shimmer, and relative slipping between the platform and character.

### Choose deterministic draw ordering

Use named layers for background, gameplay, effects, and interface. For actors sharing a layer, derive depth from the gameplay anchor, such as grounded feet, and add a stable tie-breaker. Split foreground and background pieces of tall objects when the actor needs to pass through a visual opening.

**Check** two actors crossing at equal depth and moving past a doorway, tree canopy, and translucent effect. Repeat at different speeds and after restoring a checkpoint. Verify overlap order, silhouettes, and stability on the tie frame.

### Batch compatible sprites without changing composition

Pack frequently adjacent sprite assets into atlases. Add edge extrusion and padding appropriate to sampler and mip behavior. Group draws by compatible state where blend order permits it. Cull sprite or tile groups to bounds that include the furthest overhanging artwork.

**Check** gutters, seams, and disappearing edge sprites while scrolling and zooming. Measure draw count and GPU time in a dense scene before and after batching, and compare the output for altered translucency or overlap.

### Give each pass an explicit resolution

Choose whether world sprites, masks, post-processing, and interface render at internal or output resolution. Align mask sampling to the world grid where sharp edges matter. Place interface after world scaling if small text needs independent display resolution.

**Check** moving light edges, fades, and text over the same level at every supported aspect ratio. Compare screenshots for clipped layers, blurred boundaries, and mismatch between masks and sprites.

### Keep sprite origins stable across animation frames

Store an attachment origin in the sprite's original canvas coordinates. When trimming transparent borders, save the trimmed rectangle's offset and reconstruct the origin from that offset when drawing. Give gameplay colliders and held-item sockets their own authored anchors when their position differs from the visible frame. Apply horizontal flips around the same logical origin.

**Check** an idle-to-run sequence whose frames have different transparent bounds, then repeat after atlas repacking and while facing both directions. Overlay foot and hand anchors at gameplay scale. Confirm the planted foot, held item, and hitbox move only where the animation calls for motion.
