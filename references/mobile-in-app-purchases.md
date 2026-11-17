# Mobile in-app purchases

## Problem and design reasoning

Store approval and entitlement delivery can occur in different app sessions. Supercell's *Brawl Stars* support documents delayed Android purchases, and Apple describes using transaction state to unlock game content. The practical inference is to model purchase, verification, and entitlement delivery as durable transitions. [Brawl Stars purchase support](https://support.supercell.com/brawl-stars/en/articles/purchases-i-didnt-get-my-google-android-purchase-2.html) · [Apple WWDC26, Unlock in-game content with StoreKit and Background Assets](https://developer.apple.com/videos/play/wwdc2026/378/)

## Techniques and practical checks

### Use the store’s localized offer

Show the effective price and contents, and handle unavailable products explicitly.

**Check** Test several storefronts and an unavailable item. Confirm the displayed offer matches the store sheet and can be dismissed safely.

### Distinguish pending from completed

Keep pending purchases visible and grant only after verification. Record transaction identity and delivery together so retries are idempotent.

**Check** Complete a sandbox purchase after closing the app. Deliver repeated test notifications and verify exactly one grant.

### Separate ownership and installation

Restore eligible entitlements through the store and make asset download state visible independently.

**Check** Reinstall with a purchased pack, restore ownership, and interrupt its download. Verify the player can retry without paying again.

### Reconcile reversals and failures

Define how the platform’s supported reversal events affect entitlements and explain meaningful changes to the player.

**Check** Use the platform sandbox for a cancellation, refund and temporary outage. Check state reconciliation and accessible support information.
