# Deckbuilding and card rules

## Problem and design reasoning

**Problem.** Cards lack a purpose in plausible decks, or action automation either interrupts every step or skips a legal response the player needed.

**Design reasoning.** Mega Crit describes using metrics, feedback, and difficulty tiers to iterate card purpose. Wizards documents automated priority with player-controlled stops. A design inference is to evaluate cards in context and make effect order and decision windows explicit independently of interface pacing.

## Shipped examples

**Slay the Spire (2019).** Mega Crit paired runs with deckbuilding. Anthony Giovannetti describes using metrics, playtest feedback, and difficulty tiers to iterate toward a pool in which each card has a purpose. A deck's existing cards and a player's progress can change how valuable the next addition is. [Mega Crit, GDC 2019 slides](https://media.gdcvault.com/gdc2019/presentations/Giovannetti_Anthony_SlayTheSpire.pdf)

**Magic: The Gathering Arena (2019).** Wizards documents a default priority policy that skips some pauses, explicit player-controlled stops, and special cases that retain priority for relevant interactions. The app adapts how much interaction it requests while still giving players a way to act during legal windows. [Wizards of the Coast, priority in Arena](https://magic.wizards.com/en/news/mtg-arena/announcements-october-27-2025)

## Techniques and practical checks

### Separate card definitions from individual game state

Store card IDs, targeting rules, costs, and effect definitions in authored data. Give runtime card instances their own identity and track their owner, current controller, zone, counters, and temporary modifications. Enumerate legal moves between zones and what state each move preserves. Route deck changes such as transforms and generated copies through explicit transitions.

**Check** a deck containing two copies of one card. Upgrade one copy, move it across zones, then save and reload. Verify that the correct instance retains its upgrade and that the total count changes only when an effect creates or removes an instance.

### Resolve effects in explicit order

Define the phases of a play, including target selection, cost payment, rule validation, and effect resolution. Choose an order for simultaneous triggers and a policy for newly queued triggers. Define when players can respond and when an effect revalidates its targets. Give automated passes and manual stops traceable reasons so designers can see why a response window was skipped.

**Check** two effects that trigger together, one that queues another effect, a target that becomes invalid, and a deliberate trigger loop. Compare the action log with expected outcomes and verify that the loop follows the game's termination policy.

### Make randomness reproducible and private where needed

Use seeded randomness for draws and offers, with stable ordering of inputs to random selections. Record choices and version information for debugging. In multiplayer, resolve hidden state and shuffle on the authoritative host, then send each player the card information they are allowed to see.

**Check** the same seed and actions before and after reload. Compare draw order and offers. In a local multiplayer fixture, inspect each player's view after a draw, reveal, and shuffle to confirm it contains the expected information.

### Balance cards in context

Track how often players see a card, select it when offered, and succeed after selecting it. Compare samples by difficulty, available alternatives, and existing deck strategy where possible. Use a small set of test decks for combinations that are hard to observe in live data. Interpret a high win rate in light of unlock rules, acquisition timing, and player experience.

**Check** one card that needs a specific synergy and one generally useful card. Test both in early and late draft offers, review the resulting deck plans with players, and compare the findings with the selection and outcome metrics.
