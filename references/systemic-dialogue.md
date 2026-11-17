# Systemic dialogue

## Problem and design reasoning

Several gameplay events may request speech simultaneously, and the facts that made a line relevant can change before it plays. The dialogue system needs to select a fitting response, give urgent speech room, and preserve any information the player still needs. Model line selection, playback, and story-state changes explicitly.

## Shipped examples

**Left 4 Dead (2008).** Valve describes matching facts about the world and recent history to a database of authored responses. The team wanted writers to author specific cases and fall back to general ones without editing a large conditional program. [Elan Ruskin, *AI-driven Dynamic Dialog through Fuzzy Pattern Matching*, GDC 2012](https://www.gdcvault.com/play/1015528/Do-Say-The-Right-Thing)

**Firewatch (2016).** William Armstrong and Patrick Ewing describe the dialogue system evolving from short interruptible lines to longer conversations that could restart. The player can interrupt while continuing to explore. This suggests an authoring boundary at the conversational beat, where a return to the topic can make sense in context. [Armstrong and Ewing, *Do You Copy? Dialog System and Tools in Firewatch*, GDC 2017](https://www.gdcvault.com/play/1024415/Do-You-Copy-Dialog-System)

## Techniques and practical checks

### Select from current context

Represent speech requests with event, speaker, target, and context. Filter by mandatory conditions, then rank matching responses by specificity or authored priority. Define a stable tie-break and allow an explicit silent outcome when no line fits.

**Check** one event with a specific match, a generic fallback, and no eligible response. Use a trace to verify which facts disqualified each alternative.

### Arbitrate shared playback

Give each conversation channel a budget and each request a priority and expiry. Define which classes can interrupt, queue, or drop other speech. Recheck the speaker, location, and triggering conditions before queued playback.

**Check** an urgent warning during ambient banter, two requests at equal priority, and a queued line after its target disappears. Confirm the warning and its subtitle are timely and stale lines are discarded.

### Track the conversational beat

Keep a small durable record of what the player was told and which beat is eligible to resume. Let presentation restart, bridge with a short continuation, or end if context has changed. Give any gameplay state change a stable identity and commit it at a defined point independent of replaying audio.

**Check** interrupt before and after an important reveal, then save or leave the interaction and return. Verify that required information stays accessible and that rewards or objectives update once.

### Give writers a repeatable diagnostic

Expose the event, context snapshot, candidate rules, why candidates lost, and the resulting subtitle or playback state. Log repetition and rule coverage, then give authors a way to replay the relevant state.

**Check** with an authored set containing rare conditions, repeated triggers, and a missing localization key. Verify that writers can find the winning rule, reproduce the fault, and see the intended fallback.
