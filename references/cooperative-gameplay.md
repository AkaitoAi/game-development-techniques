# Cooperative gameplay

## Problem and design reasoning

**Problem.** One player solves every challenge while a partner waits, or teammates cannot communicate their intent precisely enough to coordinate.

**Design reasoning.** Hazelight describes designing around two players who each have something to do. A design inference from Hazelight's approach and Portal 2's pings is to make both roles necessary at useful moments and let each player signal a location or plan in the game itself.

Build co-op around decisions players make together. A shared objective needs clear contributions, a way to communicate intent, and a way to recover when a partner makes a mistake.

## Shipped examples

**Portal 2 (2011).** Valve's product page describes a separate two-player campaign with dedicated test chambers. The game's update notes document ping input across controllers and keyboard and mouse, plus per-player input handling in split-screen. Shared puzzle solving depends on communicating precise locations as well as on the puzzle rules themselves. [Valve, Portal 2](https://store.steampowered.com/app/620/Portal_2/) · [Valve, Portal 2 update notes](https://www.thinkwithportals.com/blog.php)

**It Takes Two (2021).** Josef Fares describes designing mechanics around the narrative situation and around two players who each have something to do. Hazelight's developer interview explains the team's use of mechanics specific to different parts of the game. [Josef Fares, Unreal Engine developer interview](https://www.unrealengine.com/developer-interviews/it-takes-two-lovingly-marries-story-and-gameplay-together?lang=en)

**Split Fiction (2025).** Hazelight's Hannes Gille presented its final level at GDC Festival of Gaming 2026. The session description identifies the finale's distinct co-op premise as a major design challenge and discusses why the team reserved it for the end. Josef Fares and the Hazelight team also discuss building the studio's co-op games in a BAFTA presentation. [Hannes Gille, GDC 2026 session program](https://schedule.gdconf.com/search?track=AI+and+Machine+Learning) · [Hazelight team, BAFTA talk](https://www.youtube.com/watch?v=6jLUbaC4u0A)

## Techniques and practical checks

### Give both players a meaningful contribution

Assign roles with different information or abilities, and let each player act on something the other needs. Rotate responsibilities across encounters. Keep a useful task available during a partner's longer sequence where the scenario permits.

**Check** a pair with unequal experience and then swap roles. Record meaningful actions and idle time for each player. Ask both what they contributed and where they felt unable to help.

### Provide a way to express intent in play

Use world-space pings or object highlights for references to location. Add readiness indicators for shared actions when coordination depends on timing. Give both screens a clear way to distinguish who signaled and which object was intended.

**Check** a session with voice chat disabled. Ask pairs to indicate a target, signal readiness, and recover from an ambiguous ping while moving. Repeat on small displays and with mixed input devices.

### Make joint actions tolerant of bounded timing differences

Choose which interactions require both players and which can accept actions in either order. Keep early intent for a short interval when appropriate. Give latched actions an explicit cancellation rule and show both players when the joint action commits.

**Check** same-tick interactions, actions in both orders, timeout boundaries, cancellation, and a deliberately delayed local test connection. Verify that the joint action commits at most once and consumes the intended resources.

### Define how the pair recovers

Choose whether a failed move resets one player, both players, or just the interrupted interaction. Place recovery points where each player can reach the next legal action. Preserve completed shared progress according to a single checkpoint rule.

**Check** a partner falling during a joint action, entering a checkpoint alone, and joining an existing session where supported. Confirm that the pair regains a playable state without stranded roles, duplicate rewards, or objectives stuck in progress.
