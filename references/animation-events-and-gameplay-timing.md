# Animation events and gameplay timing

## Problem and design reasoning

One action may have several clocks. The simulation must decide when it commits, presentation must show contact or anticipation, and both must respond predictably to interruption. Naughty Dog describes synchronizing animations while also fitting the melee system into the wider gameplay of *The Last of Us*. A transferable inference is to make ownership explicit for each event so blend weights and cosmetic update cadence do not accidentally decide authoritative outcomes. [Anthony Newman, Unsynced: The Last of Us Melee System, GDC 2014](https://gdcvault.com/play/1021172/Unsynced-The-Last-of-Us)

## Shipped examples

- *Uncharted* and *Uncharted 2* layer partial and additive movement animations, illustrating that several clips may contribute to one rendered pose. [Travis McIntosh, Animation and Player Control, GDC 2010](https://www.gdcvault.com/play/1012451/Animation-and-Player-Control-in)
- *The Last of Us* addresses animation synchronization as part of melee implementation. Its developer talk also identifies multiplayer as an additional constraint. [Anthony Newman, Unsynced: The Last of Us Melee System, GDC 2014](https://gdcvault.com/play/1021172/Unsynced-The-Last-of-Us)

## Techniques

- Name authored markers by gameplay meaning, such as grab commit or foot contact. Keep state changes under the action's declared owner and route effects to presentation. **Check** that disabling or throttling animation evaluation preserves simulation outcomes.
- Collect markers crossed over a time interval and define what happens across looping, skipping, and reverse playback. **Check** a hitch crossing two markers and a loop seam. Verify marker order, count, and the policy for reverse playback.
- Give each action instance an identity and define cancellation before and after commitment. **Check** cancel during windup, blend into a different action, then repeat the original action. Verify that stale events cannot fire and new tokens remain valid.
- For paired motion, validate alignment at commit and use a recovery path when the target or space changes. **Check** a moving target, an obstruction, and divergent client presentation timing in a networked game. Compare presentation contact and authoritative result separately.
