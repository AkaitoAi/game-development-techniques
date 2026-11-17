# Mobile rewarded ads

## Problem and design reasoning

Rewarded ads exchange a player's time and attention for a promised in-game benefit. If the choice is unclear, declines lead to another prompt, or a network failure interrupts progression, the placement feels mandatory. Hipster Whale described opting for rewarded advertising alongside purchases and cited Disco Zoo as evidence that a voluntary model could work. Fiveamp's account identifies preserving the play experience as a priority. **Design inference.** The standard gameplay route should remain viable when players always decline, ads are unavailable, or the device is offline.

## Shipped examples

- **Crossy Road.** The GDC Free to Play Summit presentation covers rewarded video as an opt-in part of the launch monetization design. Hipster Whale's support distinguishes reward ads from autoplay ads in its shipped game. Treat the voluntary design pattern as the example, with the understanding that a game can ship other placements separately. [Matt Hall and Andy Sum, GDC 2015, Crossy Road: A Whale of a Time](https://media.gdcvault.com/gdc2015/presentations/Hall_Matthew_Crossy_Road_Whale.pdf) · [Hipster Whale support](https://www.hipsterwhale.com/crossy-road-support)
- **PickCrafter.** A developer case study describes a seasonal placement with a direct reward path and an alternative reward for choosing to view an ad. The baseline reward provides a usable continuation when the player declines. [Fiveamp case study on Unity](https://unity.com/resources/pickcrafter)

## Implementation checks

1. **Ask at a stable point in play.** Label the viewing action, the exact reward, and the normal continuation. Display the choice after the game has saved or settled the action it follows. **Check** rapid taps, repeated declines, controller or assistive navigation where applicable, and an unavailable ad. Verify the normal route remains clear and resumes promptly.
2. **Model show, completion, and return as separate events.** Treat dismissal and a verified reward outcome separately because SDK events can arrive in different orders. Keep the grant idempotent using a transaction identity and an authoritative reward ledger where the economy has a backend. **Check** duplicate callbacks, an early dismissal, a verified completion after dismissal, and a crash between completion and grant. Verify one credit for each qualifying completion and clear status for any pending reward.
3. **Make SDK failure degrade to ordinary play.** Initialize only when permitted by the player's settings and applicable platform requirements. Handle no fill, timeouts, loss of connectivity, and app switching. Release audio focus and restore game input and audio when returning. **Check** offline launch, denied consent, rotation where supported, and app suspension during a video. Verify gameplay can continue without waiting for the ad system.
4. **Measure the experience and the economy.** Compare a no-ad progression cohort with a cohort that chooses the placement. Observe time spent, reward delivery failures, dismissals, feedback, and progression changes alongside revenue. Keep the base economy viable and set a placement cap appropriate to the session. **Check** that a player can finish a representative progression segment while declining every offer and that frequent viewing stays within the intended reward budget.

For purchases and entitlement recovery, see [Monetization](monetization.md).
