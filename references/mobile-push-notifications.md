# Mobile push notifications

## Shipped examples

**Pokémon GO.** When a friend sends a raid invitation, a player may see it in-game or receive a background push if the Raid Invitations setting is enabled. Tapping a push opens the app, where an active invitation can be found in Nearby. The invitation can expire before the player arrives. [Pokémon GO help, raid invitations](https://scopelyexplore.helpshift.com/hc/en/5-pokemon-go/faq/1177-inviting-friends-to-raid-battles/)

**Clash of Clans.** Supercell added a Clan Chat Tagging notification switch scoped to each player avatar. Its release notes say that this in-game setting starts enabled and can be changed independently for different accounts. This illustrates an account preference that can coexist with the device-wide permission setting. [Supercell, Temperamental Tentacles release notes](https://supercell.com/en/games/clashofclans/blog/release-notes/temperamental-tentacles-update/)

## Problem and design reasoning

Players can miss a useful invitation while the game is closed. Stale or irrelevant alerts can also interrupt them and undermine trust. Apple engineers explain how notification delivery can respect the user's Focus settings and how interruption level signals urgency. At the GDC Mobile Summit, Martine Spaans argued that retaining players depends on game design and the player experience. The shipped examples support an inference that a push is most useful when it extends a feature the player already chose to use, with enough context to decide whether to open the game. [Apple WWDC21, Send communication and Time Sensitive notifications](https://developer.apple.com/videos/play/wwdc2021/10091/) · [Martine Spaans, GDC 2017, Mobile Gaming and the Retention Rhetoric](https://gdcvault.com/play/1024154/Mobile-Gaming-and-the-Retention)

## Techniques and practical checks

### Ask in context and separate notification categories

Offer an opt-in when the player takes an action that benefits from a reminder, such as scheduling a team activity. Explain the expected messages and leave the game usable when permission is declined. Keep account-level categories and the device permission separate in the state model. Make the way to revoke consent easy to find.

**Check** grant, deny, revoke in OS settings, and switch between two accounts. Confirm that disabling one category suppresses future sends for that category and that an OS-wide denial blocks device delivery.

### Model freshness and deduplicate at delivery

Identify each event with an account, category, stable event identity, and expiry. Match the TTL to the gameplay window. Coalesce repeated updates, set a frequency budget, and respect quiet periods. Use preview-safe text for lock screens.

**Check** deliver duplicate events, flip the device time zone, and hold a device offline beyond the expiry. Verify that stale alerts disappear and that one event produces at most one visible alert per intended delivery surface.

### Resolve the destination from current game state

Use a typed route and event identifier. After opening the app, verify the active account and fetch the current status before showing an action. Explain expiration, cancellation, or lost eligibility briefly and route to a useful in-game surface. Coordinate foreground notifications with the in-game inbox.

**Check** tap the same test alert on cold launch, in the foreground, after the invitation expires, and while signed into another account. Confirm that the player sees the right context without a stale action or account data leak.

### Measure useful outcomes and interruption costs

Log eligible events, scheduled sends, attempts, opens, completed actions, and category opt-outs with minimal account data. Keep an in-game path for the same activity. Evaluate whether the notification leads to the intended action and whether it drives unwanted interruptions or disables.

**Check** compare a pilot cohort against a holdout that keeps the in-game path. Evaluate completed useful actions alongside opt-outs, invalid destinations, and user feedback before increasing traffic.
