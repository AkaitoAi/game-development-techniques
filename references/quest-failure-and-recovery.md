# Quest failure and recovery

## Problem and design reasoning

Quests can become unsatisfiable when a character leaves, a required item is already acquired, or an event arrives in an unexpected order. The player needs to understand whether a specific opportunity is gone, another route is available, or the quest is over. A transferable inference is to separate world facts, objective status, and the final story outcome. That allows a failed objective to redirect an active quest while preserving the consequence.

## Shipped-game examples

- **Baldur's Gate 3.** Larian's journal documentation describes objective priorities, alternative subquests, and displaying skipped or failed objectives as inactive. It explicitly accounts for shortcuts such as getting a needed item before being asked. Patch 6 fixed an update when Baelen is killed before dialogue and corrected a journal instruction that pointed back to an unavailable character in another quest. These cases show how journal state must reflect out-of-order actions and world changes. [Larian journal structure](https://docs.baldursgate3.game/index.php?title=Journal_Structure_Overview) · [Larian Patch 6 notes](https://baldursgate3.game/news/patch-6-now-live_108)
- **Alien: Rogue Incursion.** Survios co-founder Alexander Silkin's talk describes quest state-machine assets with a main completion path, extra branches for organization or parallel logic, and nested quests for splitting sequences across assets. An inferred application is to define which nested results feed the parent outcome and test them independently before combining them. [Constructing the World of Purdan in Alien: Rogue Incursion, GDC 2025](https://media.gdcvault.com/gdc2025/Slides/Silkin_Alexander_Constructing%2Bthe%2BWorld.pdf)

## Implementation and practical checks

Give each vulnerable objective an explicit policy for prerequisites that disappear. For an optional objective, record the lost opportunity and close or redirect it. On a required route, define a reachable substitute, a retry boundary, or a clearly communicated end state consistent with the game's rules.

- **Check** Remove each required person, item, or location while the objective is active. Confirm that the quest closes with the intended outcome or presents a reachable next step whose availability matches the world.

Evaluate durable facts when entering a state as well as when receiving events. Treat finding an item early or learning a clue from another source as an input to eligibility. Record one-shot rewards against a stable completion identity.

- **Check** Acquire the item before accepting the quest, acquire it after acceptance, and reload between acquisition and turn-in. Confirm that equivalent playthroughs agree on outcome and payout while meaningful alternate paths remain distinct.

Derive quest log text, markers, dialogue availability, and rewards from resolved state. Store the reason when a transition retires an objective so presentation can describe lost access, refusal, or an alternate completion. Update all player-facing surfaces when the state changes.

- **Check** Trigger a failed branch and recover through a substitute route. Inspect the journal and map before and after loading the save, and confirm no text directs the player to an inactive objective or promises a removed reward.

Define how scoped quest states compose. For each nested objective, record whether completion, failure, or cancellation blocks, advances, or leaves the parent unchanged. Guard against updates from a retired quest instance and preserve the parent outcome across saves.

- **Check** Resolve nested objectives in opposite orders, including simultaneous failure and success, then reload after each outcome. Verify consistent parent status for equivalent orders and ensure completion effects run only once.
