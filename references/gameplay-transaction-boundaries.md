# Gameplay transaction boundaries

## Problem and design reasoning

One player action can affect several records. Buying an item can change a balance, stock, ownership, and quest progress. If dependent systems observe intermediate writes, the result can be inconsistent even when each subsystem behaves correctly in isolation. Define which changes must appear together, what may happen after the commit, and how the caller learns the final outcome.

CCP documents how shipped *EVE Online* matches market orders, including splitting fulfillment across orders and listing the remainder. Davíð Einarsson's GDC presentation describes the scale of the player-driven market and the operational importance of reliable accounting. An implementation inference is to model each fill as a complete state transition with explicit remaining quantity. These sources describe the market contract and operational context. [CCP, Buy and Sell Orders](https://support.eveonline.com/hc/en-us/articles/203218932-Buy-and-Sell-Orders) · [Davíð Einarsson, Addressing Exploits in EVE Online with Customer Care, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Adressing_Exploit_Abuse.pdf)

## Practical checks

- **Define the atomic unit.** List preconditions and all state changes for one action. Validate them against a consistent view and commit through the state owner. For a partial fill, commit the matched quantity and leave the remainder explicit. **Check** a full inventory, insufficient balance, and a valid partial fill. Reconcile quantities and balances against the intended result.
- **Return the same result for retries.** When a command may be retried, persist its operation identity with its outcome in the same commit boundary. Distinguish that identity from a later deliberate repeat. **Check** replay a successful request after its response is lost and verify one state change. Submit a new intentional request and verify that normal eligibility rules apply.
- **Publish consequences after commit.** Produce rewards and gameplay notifications from the committed result. If a notification must survive restart, record it alongside the change and make its consumer safe to retry. **Check** inject a failure before commit and another before notification delivery in a test build. Verify that only the committed path produces the effect and that restarting can recover delivery.
- **Define recovery across owners.** When one commit cannot cover multiple services, model reservation, settlement, and release as durable states with explicit recovery behavior. **Check** interrupt between reservation and settlement, then resume or expire it. Verify that every reserved resource reaches a terminal state and the player can see whether the action completed.

See [Trading and player markets](trading-and-player-markets.md) for player-facing market design and [Gameplay invariants and validation](gameplay-invariants-and-validation.md) for rules to assert across transitions.
