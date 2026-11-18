# Impact sound design

## Problem and design reasoning

An impact should communicate what contacted what, how forceful it was, and what happened. A physically correct contact stream can still produce repetitive chatter or mask an important break. Stephen Hodde's *Red Faction: Armageddon* talk explicitly frames physics-driven destruction audio around emotional goals, system design, and content creation. It provides shipped-game context for designing the audible result alongside the simulation. [Stephen Hodde, The Weight of the World, GDC 2012](https://gdcvault.com/play/1015604/contactUs)

The released *Quake III Arena* client selects impact sounds and marks by weapon and impact category. Its mission-pack code distinguishes flesh, metal, and default outcomes for some weapons. This is a concrete compact mapping from outcome data to presentation. The material-pair, strength, and repetition rules below are recommendations for a broader contact system. [id Software, Quake III Arena impact presentation](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_weapons.c)

## Techniques and practical checks

### Classify the contact and its outcome

Define an event carrying both surface classes, contact position, a strength estimate, and a gameplay outcome such as bounce, blocked hit, break, or scrape. Choose a principal result before adding decorative layers. Keep persistent scraping or rolling separate from a new impact onset.

**Check** drop one prop onto wood, metal, and a soft surface, then trigger a break and a bounce at comparable strength. Ask listeners to identify both surface and outcome. Verify that one contact does not also start an unintended looping sound.

### Tune intensity with bounded parameters

Map a useful physical measure such as normal impulse or approach speed into authored intensity bands. Shape gain, attack, body, and debris detail to fit the action. Cap extreme values and define a fallback when the simulator cannot provide the preferred measure.

**Check** repeat low, medium, and maximum supported impacts using the same material pair. Compare loudness and perceived weight at a fixed playback level. Include the fallback path and check that saturation does not turn every impact into the same maximum-strength sound.

### Control repeated contacts without losing significance

Use short per-pair cooldowns, onset detection, or event merging to suppress solver chatter. Allow a distinct new break or major collision to override a low-value repeat limit. Pick sample variations with a repetition rule that preserves the material's identity.

**Check** stack several props, let them settle, and then strike the pile. Log requested and played events. Verify settling becomes quiet, the new large impact remains audible, and no residual loop survives removal of the last contact.

### Validate the result in the full mix

Assign priorities based on gameplay importance and audibility. Reserve space for a critical hit confirmation or nearby break when ordinary debris is dense. Provide visual or tactile feedback for essential outcomes and preserve the player's mix settings.

**Check** replay a busy encounter with the same impacts at low and high effects settings, on small speakers and headphones. Verify critical outcomes stay recognizable, output remains unclipped, and voice limits suppress secondary detail according to the stated policy.

See [Interactive foley systems](interactive-foley-systems.md) for movement textures and [Audio voice allocation](audio-voice-allocation.md) for playback capacity.
