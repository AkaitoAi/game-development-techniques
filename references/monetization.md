# Monetization and purchase design

## Problem and design reasoning

**Problem.** A player cannot tell the final charge, which item it grants, or what to do if payment succeeds and the item never appears.

**Design reasoning.** Rare's separate coin and cash flows and Epic's separate refund paths show why the confirmation and receipt need different facts. Design each step around charges, fulfillment, and recovery that a player can verify.

## Shipped examples

**Sea of Thieves (2018).** Rare documents two ways to buy items from the Pirate Emporium. On the web, purchase confirmation shows the Ancient Coin cost and the player's balance. The local-currency in-game flow can buy a suitable coin pack and place unused coins in the player's wallet. This makes the cash charge, item price, and residual balance three distinct facts a purchase screen should disclose. [Rare's Pirate Emporium FAQ](https://www.seaofthieves.com/pirate-emporium) · [Rare's web purchase walkthrough](https://support.seaofthieves.com/articles/22425750758930-Making-purchases-in-the-Pirate-Emporium-via-the-Sea-of-Thieves-website)

**Fortnite (2017).** Epic documents cancellation and return options for eligible V-Bucks cosmetics. Direct-money purchases follow a platform-dependent refund path. The purchase medium can determine who handles recovery, so the item history should expose that information and the next action. [Epic's purchase cancellation guide](https://www.fortnite.com/cancel-purchase?lang=en-US)

**Path of Exile (2013).** Chris Wilson's GDC 2019 talk describes how Grinding Gear Games structured ongoing development around predictable seasons, scoped releases, and replayable content. For a game that funds ongoing development, use a release plan the team can sustain and define the boundaries of each offer separately from the gameplay schedule. [Chris Wilson, GDC 2019](https://www.gdcvault.com/play/1025784/Designing-Path-of-Exile-to)

## Techniques and practical checks

### Show the complete cost before confirmation

Show the recipient account, item or bundle contents, currency, cash due now, and the available balance. For a currency top-up, disclose the pack size and amount left after the item purchase. If an offer renews, show the renewal interval, future charge, and cancellation path. Flag owned items and applicable compatibility limits before payment.

**Check** several regions and currencies with an exact balance, a short balance, and a partly owned bundle. Ask someone to state what will be charged and what they will receive from the final screen before confirming. Compare their answer with the store authorization and receipt.

### Keep browsing, preview, and consent distinct

Give browsing and preview their own actions. Use a clearly labeled purchase control and let the player back out before authorization. Restore focus predictably if the store overlay closes. Keep balance or card changes tied to the recorded transaction and provide an accessible confirmation that names the purchased items.

**Check** on mouse, keyboard, and gamepad. Hold the accept button while opening the confirmation, cancel at each stage, disconnect during the platform overlay, and try a second purchase immediately afterward. Verify each charge corresponds to the player's explicit confirmation.

### Reconcile verified transactions and entitlements

Distinguish authorization, settlement, and delivery. Record a platform transaction identifier and query the provider after ambiguous outcomes. Grant an item from verified transaction state, using one durable grant per transaction and line item. Derive restoration from recorded entitlements where the platform supports it. [Valve's Steamworks microtransactions implementation guide](https://partner.steamgames.com/doc/features/microtransactions/implementation)

**Check** in a test environment with a duplicate notification, a timeout after settlement, and a crash after recording the transaction but before showing confirmation. Verify that reopening the game recovers the entitlement once and that its history shows the correct status.

### Make refunds and recovery discoverable

Show an itemized purchase history, platform, transaction status, and the route for cancellation or support. Model a reversal explicitly and define what happens if the item was equipped, used, or part of a bundle. Reconcile revoked entitlements and currency changes without altering unrelated purchases.

**Check** an accidental purchase, an ineligible return, and a provider reversal while the game is offline. Verify that the player can find the right next step and that wallet, inventory, and receipt history agree after reconnecting.
