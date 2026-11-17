# Ability systems

## Problem and design reasoning

Abilities become hard to balance and debug when input, cooldowns, effects, and animation each make their own decision about whether an activation happened. Blizzard's shipped *Overwatch* provides a case study in scripting weapons and abilities for networked play. Its *Overwatch 2* talk on Kiriko's ultimate shows the production work behind presenting a complex ability. A transferable design inference is to define the activation outcome once and let animation, UI, and audio present it. [Blizzard, Networking Scripted Weapons and Abilities, GDC](https://gdcvault.com/play/1024653/Networking-Scripted-Weapons-and-Abilities) · [Blizzard, Building Kiriko's Ultimate Ability, GDC](https://gdcvault.com/play/1028941/)

## Techniques and practical checks

For ongoing modifiers, see [Status effects](status-effects.md). For interactions between separate actions, see [Ability combos](ability-combos.md).

- Define eligibility, targeting, cost, commit point, and recovery for every ability. **Check** insufficient resources, a target leaving range, and interruption immediately before and after commit. Verify exactly when the cost and cooldown apply.
- Keep persistent unlocks separate from a character's equipped abilities and current charges. **Check** unlock, equip, use, respec, and reload. Verify the player keeps only the abilities and charges permitted by the rules.
- Describe effect duration, stacking, replacement, and removal explicitly. **Check** overlapping buffs, repeated hits from the same source, two different sources, and expiration on the same tick. Verify the result matches the stated priority.
- Give activation, success, failure, and interruption distinct feedback. **Check** a cancelled cast and a successful cast with reduced effects and muted audio. In multiplayer, also verify a delayed confirmation does not duplicate feedback or grants.
