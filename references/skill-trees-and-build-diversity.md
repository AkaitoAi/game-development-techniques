# Skill trees and build diversity

## Problem and design reasoning

A wide tree creates little practical variety if every build relies on the same multiplier or reaches its defining ability near the end of play. A tree should make reachable alternatives legible and leave a way to recover from misunderstood choices. Grinding Gear Games documents class-specific starting positions, recognizable Notables, rule-changing Keystones, and a refund policy designed to allow small corrections while preserving larger character commitments. An inference for other games is that distinct mechanics and visible milestones can matter more than the number of nodes. [Grinding Gear Games passive skill tree](https://www.pathofexile.com/passive-skill-tree)

## Shipped examples

- **Path of Exile (2013).** Classes start at different points on a shared passive graph. A player can specialize locally or spend points traveling to another region. Keystones can change a rule, such as who receives a shield's properties. At GDC 2019, Chris Wilson discussed deep gameplay systems as part of the game's strategy for supporting long-term engagement. [Official passive tree](https://www.pathofexile.com/passive-skill-tree) · [Chris Wilson, GDC 2019](https://www.gdcvault.com/play/1025784/Designing-Path-of-Exile-to)
- **Diablo IV (2023 launch design).** Blizzard presented class trees with skills, enhancements, passive skills, and Key Passives. Its developer overview also describes gear bonuses as another way to tailor a build. Testing the interaction between tree and gear is therefore more representative than comparing tree nodes in isolation. [Blizzard developer overview](https://news.blizzard.com/en-us/article/23938756/make-sanctuary-yoursplay-your-way-in-diablo-iv)

## Techniques and practical checks

1. **Validate graph legality and cost.** Give nodes stable IDs, define prerequisites and exclusions as data, and validate affordability and connectivity in one place. Use the same rules for previews, allocation, and refunds. **Check** representative roots, locked branches, cross-branch routes, and refunding a prerequisite. Verify invalid edits keep the point balance intact.
2. **Design branches around different play patterns.** Give paths distinct decisions such as timing, range, resource use, or targeting. Place the first defining effect early enough that a player can try it during normal progression. **Check** representative builds under identical point and gear budgets. Compare action sequences and outcomes across several encounter shapes.
3. **Measure diversity in context.** Track selection and outcomes by progression band, content type, and relevant gear. Review high-pick nodes for mandatory utility, interface prominence, or easy access before changing their values. **Check** an optimized build, a plausible beginner build, and a niche build at early and late checkpoints. Record performance and which decisions each path makes available.
4. **Design refunds and migration together.** Preview affected nodes, changes to equipment eligibility, and returned points before a reset. On a layout update, remap stable IDs or refund obsolete selections with a clear explanation. **Check** upgrading an old save with removed nodes and dependent equipment, abandoning a respec midway, and repeating a migration. Confirm consistent point totals and a playable character.

## Verify that unlocks reach gameplay

Give each node a stable reward ID. Recompute the selected build from purchased nodes whenever a loadout changes. Keep unlock ownership separate from equipment slots and current cooldowns. **Check** buy a prerequisite and its dependent node, equip the ability, then refund the prerequisite and reload. Verify that dependent nodes follow the stated refund policy and that the loadout and UI update together. See [Ability systems](ability-systems.md).
