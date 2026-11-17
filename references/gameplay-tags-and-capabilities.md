# Gameplay tags and capabilities

## Problem and design reasoning

Rules such as “can use a traversal ability while carrying this item” become brittle when every caller knows every actor class or status effect. Tags give systems a shared vocabulary for classification and eligibility. A capability is an operation with a contract, such as checking whether an actor can carry a specific object and attempting the transfer. A tag can help discover candidates for an operation, while the capability validates the current actor, target, and context.

Epic documents hierarchical tags and tag queries for gameplay in *Fortnite* Creative, where a Verse script can find multiple tagged devices or entities without assigning an individual reference to each. Blizzard's Timothy Ford describes using ECS to support varied hero features in shipped *Overwatch* with manageable complexity. The inference for a custom game is to compose behavior through explicit contracts and use tags for selections that designers need to author. Blizzard's talk documents component architecture. [Epic, Verse Tags in Fortnite](https://dev.epicgames.com/documentation/fortnite/verse-tags-in-fortnite) · [Timothy Ford, Overwatch Gameplay Architecture and Netcode, GDC 2017](https://gdcvault.com/play/1024001/-Overwatch-Gameplay-Architecture-and)

## Practical checks

- **Define query semantics.** Give each tag family an owner and document exact matches, ancestor matches, and all/any/none predicates. **Check** that a query for a parent matches its descendants only where intended, an empty query has explicit semantics, and mutually exclusive tags cannot be authored together.
- **Give temporary tags a source.** If multiple effects grant the same tag, track those grants independently and derive active membership from them. **Check** apply the same tag from two sources, remove them in reverse order, and verify that eligibility changes only when the last source expires.
- **Validate capabilities at use time.** Use tags to narrow candidates, then have the receiving capability check current requirements and return a useful success or rejection result. **Check** remove a capability or change the target after selection but before activation. Confirm the attempted action is rejected consistently and produces no side effects.
- **Version the vocabulary.** Keep stable names or IDs for persistent and networked tags, and define what happens when a name is renamed or removed. **Check** load an older save or authored fixture after a rename. Verify that known aliases resolve and unsupported tags receive the documented fallback or validation error.

See [Entity-component-system design](entity-component-system-design.md) for behavior composition and [Ability systems](ability-systems.md) for activation lifecycle.
