# Checkpoints and respawn design

## Problem and design reasoning

A retry can support learning or add repetition between useful attempts. Decide what the player should reconsider after failure and which completed actions are worth replaying. Team Meat explicitly paired short levels and quick respawns with removing lives to support repeated attempts. A designer inference from Ori's limited player-placed checkpoints is that safe placement can become a resource decision when exploration is central. [Team Meat postmortem](https://www.gamedeveloper.com/audio/postmortem-team-meat-s-i-super-meat-boy-i-) · [Xbox Wire gamescom demonstration](https://news.xbox.com/en-us/2014/08/14/gamescom-ori-preview/)

## Shipped examples

- **Super Meat Boy (2010).** Edmund McMillen describes reducing respawn delay and keeping the goal visible as part of the team's approach to a demanding platformer. The team also presented the game's development at GDC 2011. [Developer postmortem](https://www.gamedeveloper.com/audio/postmortem-team-meat-s-i-super-meat-boy-i-) · [Team Meat, GDC 2011](https://gdcvault.com/play/1014684/Team-Meat-Presents-SUPER-MEAT)
- **Ori and the Blind Forest (2015).** Soul Links let players create respawn points at nearly any safe place, within a limited-use system. Checkpoint placement therefore enters the player's planning for hard stretches. [Xbox Wire launch coverage](https://news.xbox.com/en-us/2015/03/12/games-ori-and-the-blind-forest-is-a-sight-to-behold/) · [gamescom 2014 demonstration](https://news.xbox.com/en-us/2014/08/14/gamescom-ori-preview/)

## Techniques and practical checks

1. **Place boundaries around meaningful attempts.** Group actions that belong to the same challenge. Place fixed checkpoints on safe ground before an attempt with enough distance to read the next threat. **Check** first-time players and repeated failures. Record time until control returns, time until the failed action is attempted again, and unrelated actions replayed.
2. **Specify the reset contract.** For each actor, gate, reward, consumed resource, and scripted trigger, define whether it restores, persists, or is recomputed. Commit the intended checkpoint record before acknowledging activation to the player. **Check** death before and after opening a gate and collecting its reward. Verify progress, resource counts, and trigger eligibility after each retry.
3. **Validate candidate respawn sites.** For player-placed points, test clearance, hazard exposure, and reachability of required exits. Define what happens if later world changes make a saved site unsafe. **Check** a point near moving hazards and a site invalidated by a rearranged scene. Verify that the fallback is safe and routes back into play.
4. **Make retry restore the full control loop.** Reset stale input, camera overrides, temporary status effects, and active dialogue or animations as appropriate. Handle repeated death notifications and retries arriving during scene loading. **Check** a held movement button, a defeat during a scripted camera move, and consecutive rapid retries. Verify one spawn per retry and usable camera and input on return.
