# Collectible and secret design

## Problem and design reasoning

Players need enough information to recognize an opportunity and enough uncertainty to enjoy discovering it. Required pickups also need a reliable route to completion, while optional challenges can ask for more effort. Nintendo describes Power Moons as the fuel for travel in Super Mario Odyssey and provides optional help for finding them. Maddy Thorson's Celeste talk describes designing individual stages and arranging them into larger areas. A design inference is to place and tune a collectible in the context of how the player approaches, retries, and leaves its room. [Nintendo, Explore the Kingdoms](https://supermario.nintendo.com/explore/) · [Maddy Thorson, GDC 2017](https://www.gdcvault.com/play/1024307/The-Gamer-s-Brain-How)

## Shipped examples

- **Super Mario Odyssey (2017).** Power Moons fund travel, and Nintendo's guide points to unusual spots in the landscape as invitations to investigate. The game also offers characters that provide hints. These layers let players choose between exploration and more explicit guidance. [Nintendo, secrets and tips](https://play.nintendo.com/news-tips/tips-tricks/super-mario-odyssey-secrets-tips-tricks/) · [Nintendo, adventure features](https://www.nintendo.com/my/switch/aaac/adventure/index.html)
- **Celeste (2018).** Celeste tracks strawberries in the journal, with special handling for a Golden Strawberry in a chapter without ordinary strawberries. Thorson's *Designing Celeste* workshop discusses the process behind individual platforming rooms and the arrangement of broader area maps. Together these sources support treating route design and completion reporting as connected concerns. [Celeste developer changelog](https://www.celestegame.com/changelog.html) · [Maddy Thorson, GDC 2017](https://www.gdcvault.com/play/1024307/The-Gamer-s-Brain-How)

## Techniques and practical checks

### Define the role of each pickup

Classify each pickup by its purpose, such as required progression, optional mastery, narrative discovery, or cosmetic completion. Set difficulty and checkpoint rules accordingly. When a progression gate accepts any subset, confirm multiple sets can satisfy it.

**Check** one route that skips all optional challenges and another that collects mainly difficult pickups. Verify each reaches the intended gate, and check that the optional route offers a meaningful choice.

### Signal the point of investigation

Use landmarks, framing, repeated environmental patterns, or sound to suggest that a location is worth inspecting. Choose whether a hint reveals an area, a specific object, or the action required. Consider a stronger optional hint for players who return repeatedly.

**Check** with first-time players before showing them a map or solution. Record when they notice the clue, their first interpretation, and whether the optional hint helps them act.

### Design recovery and return routes

Give each risky collectible an intentional retry location and preserve an exit from optional side paths. Decide explicitly whether completion persists on pickup, return to safety, or crossing a checkpoint.

**Check** collect the item and fail immediately, leave the area before collecting, and revisit after finishing the level. Verify progress and respawn behavior match the promise made by the interface.

### Keep collection state stable and legible

Use stable content IDs and calculate completion from the items available to the player in the relevant region, game version, and mode. Define how hidden categories and expansion items appear in counters.

**Check** reload a save after moving an existing collectible, finish a region without hidden challenge items, and install optional content. Verify the counter, pickup state, and rewards agree and that revisiting cannot award the same item twice.
