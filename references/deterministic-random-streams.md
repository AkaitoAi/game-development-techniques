# Deterministic random streams

## Problem and design reasoning

A repeatable pseudorandom generator returns the same sequence when initialized the same way, but changing which subsystem draws from it can change every subsequent outcome. Stable streams let selected systems vary independently and let developers replay the randomness that affected gameplay. Seed derivation, draw ordering, algorithm version, and saved state all matter.

*Factorio* documents an independently seeded random generator for scripts that is separate from the core game generator. Wube also explains how deterministic lockstep depends on reproducing simulation from inputs. An engineering inference is to assign random draws to the gameplay owner that should reproduce them and keep cosmetic draws outside that owner’s stream. [Wube, LuaRandomGenerator runtime documentation](https://lua-api.factorio.com/latest/classes/LuaRandomGenerator.html) · [Wube, Friday Facts #188](https://www.factorio.com/blog/post/fff-188)

Ubisoft’s Julien L’Heureux documented that shipped *Rainbow Six Siege* seeded a generator from an impact position for procedural destruction, with agreement on inputs and event order as dependencies. This illustrates an event-scoped approach. An event-keyed design can avoid shared draw-order dependencies when every participant has the same canonical event data. [Julien L’Heureux, The Art of Destruction in Rainbow Six: Siege, GDC 2016, Randomness vs. Determinism slides](https://media.gdcvault.com/gdc2016/Presentations/LHeureux_Julien_Art_Of_Destruction.pdf)

## Techniques and practical checks

- Partition streams by stable gameplay purpose where independence is required. Derive stream seeds with a documented mixing function over the root seed and canonical, distinct stream identifiers. Avoid platform-provided hashes whose values can change across runs. **Check** insert a cosmetic draw and then an extra encounter draw. Loot and other unrelated streams should still produce the expected sequence.
- Record the generator version and sufficient state to resume each stream in snapshots and saves. A root seed is sufficient only when the complete draw history can be reproduced from that point. **Check** checkpoint after a variable number of draws, reload, and compare the next sequence against an uninterrupted run. Exercise a saved run from the previous supported version.
- For parallel or unordered work, derive random samples from stable event keys, or give each independent job its own stream. Encode identifiers and numeric inputs in a canonical form before deriving keys. **Check** process the same event set in reverse order and with different worker counts. Compare each event’s result by ID and verify repeated processing follows the intended replay rules.
- Test determinism and statistical behavior separately. Pin known-answer vectors for seed derivation, raw draws, and bounded sampling, then check distributions and unwanted cross-stream correlation over many seeds. **Check** run the vectors on supported builds, and compare an action trace by tick and stream when it diverges. Inspect the first mismatched state or draw count.
