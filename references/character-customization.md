# Character customization

## Problem and design reasoning

**Problem.** A large option set overwhelms players, independently valid parts clip when combined, or existing saves lose options after content changes.

**Design reasoning.** Maxis describes layering rules and direct manipulation in The Sims 4. Capcom demonstrates choosing a nearby preset before refinement. A design inference is to provide a quick way into the editor while using compatibility metadata to keep its combinations reliable.

## Shipped examples

**The Sims 4 (2014).** Maxis described a system for defining how assets that cover the same physical area layer over one another. The team also demonstrated direct manipulation, where players drag a region of the character to adjust its shape. Tools for combining assets and controls for editing them solved different parts of the same authoring problem. [Maxis, Innovations in The Sims 4 Character Creator, GDC 2015](https://www.gdcvault.com/play/1022085/Innovations-in-The-Sims-4)

**Dragon’s Dogma 2 (2024).** Capcom’s showcase described a creator that uses photogrammetry and lets players narrow down an appearance by selecting the closest of several images before refining it. The creator serves both the player character and the player’s main Pawn. Preset selection gives players a quick starting point for the same feature set that supports detailed editing. [Capcom showcase details](https://blog.playstation.com/2023/11/28/dragons-dogma-2-arrives-march-22-2024-on-ps5/)

## Techniques and practical checks

### Give slots and shape parameters stable identities

Represent the design as selected asset IDs, bounded shape parameters, and material values. Define which regions each asset covers and how it interacts with layered assets. Specify when equipment masks underlying geometry, substitutes a compatible piece, or stays visibly layered. Store semantic values in saves and network messages so equipment catalog reordering does not change an existing character.

**Check** an old save after an asset is renamed, moved, or removed. Verify that the original appearance stays intact wherever a replacement exists and that a missing part falls back predictably. Equip and unequip overlapping pieces in both orders and compare the resulting outfit.

### Define a supported deformation envelope

Use bone adjustments for large proportional changes with a rig designed for them, and use mesh deformation for local shape changes. Fit clothes and accessories against the same shape changes or limit combinations to supported ranges. Track attachment positions, skin weights, and deformation conventions across mesh detail levels and retargeted clips.

**Check** representative boundary combinations, including extreme proportions together, while walking, sitting, turning, and speaking. Inspect clip-through, seams, attachment offsets, and silhouettes at each detail level. Include the actual gameplay and dialogue cameras.

### Provide a quick path and a precise path

Offer curated presets or constrained randomization for fast entry. Keep reset, undo, and numeric or discrete adjustment available when a drag control is difficult to target. Use preview lighting and viewpoints that represent the shipped palette and camera range.

**Check** time to create a character from a preset and time to reproduce a reference using detailed controls. Try keyboard, pointer, and gamepad where supported. Reopen the creator after saving and verify that displayed selections match the resulting appearance.

### Budget crowds of unique characters

Measure the cost of visual diversity across geometry, textures, material variants, animation, and network payload. Reuse compatible base assets and apply cheaper customization in distant views where it preserves important silhouettes and identifiers. Bound and validate customization data before constructing an avatar.

**Check** worst-case scenes with unique outfits and frequent joins or swaps. Track memory, draw submission, loading time, and appearance pop-in. Verify that the least expensive representation still distinguishes characters whose identity affects play.
