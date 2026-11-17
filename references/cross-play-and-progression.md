# Cross-play and cross-progression

## Problem and design reasoning

**Problem.** Players can match across platforms yet cannot form parties, or linking accounts threatens existing progress and platform-specific purchases.

**Design reasoning.** Psyonix staged matchmaking and parties separately and documented a primary-platform choice for progression. Bungie describes coordination across teams and platform requirements. A design inference is to define interoperability and authoritative progress independently, with explicit ownership exceptions.

## Shipped examples

**Rocket League.** Psyonix enabled matchmaking across PlayStation 4, Switch, Xbox One, and Steam in January 2019. The announcement distinguished cross-platform matchmaking from cross-platform parties, which followed later. In 2020, the studio described a primary-platform choice for sharing rank, pass progress, and XP alongside explicit exceptions for platform-specific items and balances. [Psyonix cross-play launch](https://www.rocketleague.com/news/full-cross-platform-play-now-live-in-rocket-league?lang=en) · [Psyonix progression overview](https://www.rocketleague.com/news/cross-platform-progression-with-free-to-play--a-closer-look/)

**Destiny 2.** Cross Play launched in 2021. Bungie's GDC talk describes coordinating engine, interface, service, and web teams, as well as requirements from first-party platforms. Bungie's Cross Save guide documents a separate selection of the active character set and rules for which content remains tied to a platform. [Bungie launch update](https://www.bungie.net/7/en/News/Article/50655) · [Jon Chu, GDC 2022](https://media.gdcvault.com/GDC%2B2022/Speaker%2BSlides/Bringing%2BPlayers%2BTogether_Chu_Jon.pdf) · [Bungie Cross Save Guide](https://help.bungie.net/hc/en-us/articles/360049498031-Destiny-2-Cross-Save-Guide)

## Techniques and practical checks

### Separate game identity from platform profiles

Use a stable game-account key and link platform identities through their verified sign-in flows. Show the profiles and progress set the link would affect before committing. Define the outcome of collisions, unlinking, and support-assisted recovery. Maintain the audit history needed to resolve accidental links.

**Check** two profiles with the same visible name, a profile already linked elsewhere, an expired sign-in, and an interrupted confirmation. Verify that retry cannot redirect another player's profile or repeat a completed link.

### Specify play eligibility independently

Evaluate client compatibility, playlist rules, cross-play preference, and applicable platform permissions for each member of a party. Re-evaluate when membership or version changes. Use distinct messages for a blocked invitation, an incompatible build, and a temporarily unavailable service.

**Check** mixed-platform parties across each supported platform pair, including an opted-out player, a restricted account, and a client waiting for a patch. Confirm that both invite and matchmaking paths apply the same rules.

### Make progression ownership visible

List each data class and decide whether it follows the game account, an explicitly selected profile, or the purchase platform. Show the choice and its effect before linking. Give mutations identifiers and serialize or version writes that compete across devices. Reconcile entitlement grants separately from save-state selection.

**Check** accounts with competing character sets, same-session rewards recorded on two devices, platform-only content, and refunds. Verify that linking or switching preserves the intended progress and displays unavailable items accurately.

### Roll out and recover by capability

Treat friends, parties, shared matchmaking, and cross-progression as distinct capabilities with explicit dependencies. Stage each capability behind a rollout switch where supported. Track failure rates by platform pair and feature. Define what is available if one shared service becomes temporarily unavailable.

**Check** with one platform delayed, a failed account lookup, and an interrupted first migration. Confirm that a retry is safe and that the player can still reach permitted gameplay or a clear recovery message.
