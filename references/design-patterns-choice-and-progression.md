# Game design patterns for choice and progression

## Problem and design reasoning

Players need to understand their choices, anticipate what they give up, and see how today's decision changes tomorrow's play. A pick rate can reveal an imbalance, but it can also reflect an unclear description, unlock order, or the contexts in which a choice appears. Mega Crit describes combining metrics and community feedback while iterating on the balance of *Slay the Spire* during Early Access. Grinding Gear Games describes using overlapping axes of randomness, deep systems, and predictable seasonal releases in *Path of Exile*. The transferable inference is to design for context-dependent choices and verify that the surrounding game supplies enough varied situations for those choices to matter. [Anthony Giovannetti, Slay the Spire Metrics Driven Design and Balance, GDC 2019](https://www.gdcvault.com/play/1025731/-Slay-the-Spire-Metrics) · [Chris Wilson, Designing Path of Exile to Be Played Forever, GDC 2019](https://www.gdcvault.com/play/1025784/Designing-Path-of-Exile-to)

## Pattern catalog

The entries below describe design options and their trade-offs. The conference cases motivate the evaluation approach.

| Pattern | Problem | Approach | Trade-off |
|---|---|---|---|
| Opportunity cost | Players buy every upgrade as soon as it appears | Make multiple useful options compete for one scarce budget | A mistaken purchase can stall progress unless recovery exists |
| Sidegrades | A stronger item makes older items obsolete | Give each option a distinct strength, weakness, and use case | Comparison takes more thought and the interface must explain context |
| Specialization | Generalist builds feel identical | Reward commitment with unique tools, roles, or conditional bonuses | Highly specialized builds may fare poorly in encounters outside their niche |
| Synergy | Individual upgrades feel isolated | Let compatible choices reinforce each other or unlock a new interaction | Multiplicative combinations can dominate the original decision space |
| Counterplay | A successful strategy gives opponents no meaningful response | Offer readable setup information and more than one response where the game allows | Strong counters can turn a flexible matchup into a binary result |
| Draft | A solved build repeats every run | Present a bounded subset of options and let players adapt to what appears | Offer variance may leave particular strategies starved for support |
| Push your luck | Choosing when to stop feels arbitrary | Let players bank a visible reward or risk it for a larger return | Poorly communicated odds make losses feel unfair |
| Resource conversion | A useful resource becomes dead inventory | Provide a controlled conversion into a different resource or purpose | Cyclical conversion rules can erase scarcity |
| Resource faucet and sink | Currency keeps accumulating without useful ways to spend it | Model where currency enters and leaves the system and tune both over realistic play | Excessive sinks can penalize casual players or constrain experimentation |
| Diminishing returns | Repeated investment in one stat crowds out diversification | Reduce the marginal gain as investment rises | An opaque curve can make displayed upgrades misleading |
| Unlock gates | New players face too many systems simultaneously | Unlock a mechanic when the player has its prerequisites and a place to use it | Experienced players may want an accelerated path |
| Branching skill tree | A linear track gives no room for distinct builds | Offer mutually meaningful branches, clear prerequisites, and previewable outcomes | Deep branches magnify early mistakes if respecialization is inaccessible |
| Respecialization | A player learns their preferred build after committing | Offer a reset, refund, or loadout option with clear consequences | Easy switching can reduce commitment if long-term specialization is a goal |
| Vertical progression | Repeated play gives little visible improvement | Increase capability against content whose demands rise over time | Power inflation can trivialize older content |
| Horizontal progression | Extra stats are eroding the original challenge | Unlock alternative verbs, items, or roles within comparable power bands | Some players may miss a straightforward sense of growth |
| Run-local progression | A short session starts and ends with the same tactical options | Earn upgrades that change choices for the rest of the run | An early boost can snowball and make later decisions feel preordained |
| Meta-progression | Failure wipes out every sign of investment | Carry knowledge, unlocks, or selected resources between attempts | Permanent power gains can blur which outcomes reflect mastery |
| Duplicate protection | Repeated collection rewards cease to support the goal | Reroll duplicates, progress a guaranteed milestone, or convert extras to useful currency | A guarantee changes long-run scarcity and value |
| Catch-up mechanics | A gap makes recovery unlikely and continued play feel futile | Give trailing players or returning cohorts a bounded route back into contention | Overcorrection can reward deliberate underperformance |
| Optional objectives | Players feel forced to complete activities irrelevant to their goal | Offer parallel paths toward an equivalent milestone | Unequal effort can produce one dominant path or make rewards feel inconsistent |

## Practical checks

**Check** choice quality with matched scenarios. Give three options to players facing different resource, encounter, or loadout conditions. Record the selected option, the stated reason, and later performance. Investigate a universal choice separately from whether its benefits are clearly communicated.

**Check** the economy with short, average, and long play histories. Track every grant, conversion, and spend. Model balances, time to a target purchase, and the upper bound of repeated gains. Verify that a player can recover from an ordinary early spending mistake.

**Check** build diversity at equal progression. Run several viable builds through encounters with different demands and record time, success, and decision variety. Verify that an unusual synergy has a recognizable cost or weakness and that any promised respecialization preserves the correct unlocks.

**Check** progression across boundaries. Test a new profile, a returning player, a failed run, and a seasonal reset where supported. Compare what the interface promises with what persists after reload. Verify players can distinguish run rewards, permanent unlocks, and optional objectives.

For implementation, see [Economy and progression](economy-and-progression.md), [Skill trees and build diversity](skill-trees-and-build-diversity.md), and [Roguelike run design](roguelike-run-design.md).
