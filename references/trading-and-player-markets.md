# Trading and player markets

## Problem and design reasoning

Players need to exchange items without coordinating schedules or resolving trust by hand. A market reduces that friction, but easier acquisition can change what players spend their time doing. Design the transaction and the intended progression loop together. Use the economy guide for overall resource supply and demand, and this guide for listing, settlement, and recovery. [Economy and progression](economy-and-progression.md)

## Shipped examples

**EVE Online** implements a decentralized market with buy and sell orders. CCP describes most items as player-manufactured and prices as supply-and-demand outcomes. CCP also documented a relisting fee intended to reduce volatility. An inference for a new game is that orders suit a design where market participation is part of progression, provided order upkeep stays manageable. [Davíð Einarsson, CCP Games, GDC 2018, market overview slides](https://media.gdcvault.com/gdc2018/presentations/Adressing_Exploit_Abuse.pdf#page=3) · [CCP, Broker Relations](https://www.eveonline.com/news/view/broker-relations)

**Diablo III** shipped auction houses and later removed them. Blizzard stated that convenience and security motivated their introduction. The studio concluded that the auction houses undermined the loop of defeating monsters to obtain loot. This makes the purpose of the acquisition loop a criterion for deciding which items can be traded. [Blizzard, Diablo III Auction House Update](https://news.blizzard.com/en-gb/article/10974978/diablo-iii-auction-house-update)

## Techniques and practical checks

### Choose a mechanism and a scope

Use direct exchange for simultaneous trades, fixed-price listings for asynchronous sales, or an order book when buyers and sellers both benefit from setting offers. Decide which items and progression tiers are eligible, what quantities can fill, and whether pickup is local or global.

**Check** a scripted week of progression with and without trading. Compare time spent earning items, time spent shopping, completion time for key goals, and whether a player who prefers earning their own gear still has a viable route.

### Make settlement an authoritative state transition

Reserve eligible assets or funds when the order requires commitment. Resolve inventory, balances, order status, and receipts as one transaction or a recoverable operation with equivalent guarantees. Use durable operation identifiers so repeating a confirmed request returns its existing outcome.

**Check** partial fill followed by cancellation, two matching attempts at the same time, and a lost response after confirmation. Reconcile total items, balances, fees, and reservations. Confirm each completed exchange produces one receipt.

### Show the terms before committing

Show item identity, quantity, unit price, total price, fees, delivery location, and finality. Define a clear response when offers move between preview and confirmation. Keep quote expiry, price reapproval, and listing expiration legible to the player.

**Check** a changing quote, an unavailable item, a quantity with rounding, and a nearly full inventory. Verify the confirmed receipt agrees with the presented terms and unavailable outcomes leave funds and items recoverable.

### Instrument and operate the market

Track fill rates, time to fill, bid and ask spread where relevant, cancellations, seller concentration, and failed deliveries by market segment. Preserve a trace from player-visible receipt to settlement. Define how to pause affected listings, reconcile state, and explain corrections.

**Check** a simulated supply decrease and a planned interruption. Review the impact on casual buyers and sellers, verify pending orders remain in a defined state, and resume settlement while confirming no order is delivered twice.
