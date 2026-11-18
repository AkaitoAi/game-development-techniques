# Melee hit detection

## Problem and design reasoning

A fictional melee action needs a clear relationship between the authored swing, eligible targets, and the time when the hit becomes valid. A per-frame overlap can miss a fast motion or apply the same outcome repeatedly. *Quake II*'s released `fire_hit` routine performs a range check and a collision trace for impact attacks before applying the result. It illustrates an explicit gameplay query rather than depending solely on persistent contact callbacks. [id Software, released impact attack code](https://github.com/id-Software/Quake-2/blob/master/game/g_weapon.c)

Santa Monica's Mihir Sheth explains how *God of War* (2018) reworked combat around a closer camera and a more deliberate experience. The GDC 2019 presentation includes melee targeting among the systems affected by that perspective. A transferable inference is to tune target selection and contact validation together so the intended action remains legible from the gameplay camera. The following swept-volume recipes are implementation recommendations, not claims about Santa Monica's collision implementation. [Mihir Sheth, Evolving God of War's Combat for a New Perspective, GDC 2019](https://media.gdcvault.com/gdc2019/presentations/Sheth_Mihir_EvolvingCombat.pdf)

## Techniques and practical checks

### Make active windows explicit

Give an attack instance a stable identity, an active interval on the gameplay clock, and rules for interruption. Animation markers may request window changes, while the authoritative action state defines whether they are accepted. Preserve behavior when a hitch crosses both the opening and closing marker.

**Check** interruption before, during, and after the interval, including a skipped visual frame. Verify that a cancelled action cannot reopen its window through a delayed animation event.

### Query the motion between samples

Use a simple authored hit shape or a bounded set of samples along the animated motion. Sweep between previous and current sample transforms where the collision system supports it. Specify how angular motion and initial overlap are treated, and keep query masks consistent with the game's targeting rules.

**Check** thin targets, a rapidly turning actor, an obstacle between actors, and a target moving across the swing. Compare debug volumes with the visible motion at low and high render rates.

### Track hit policy per attack

Define whether one attack can affect a target once, once per phase, or repeatedly at a stated interval. Store hit history by target identity and attack instance. Deduplicate separate colliders belonging to the same gameplay target before resolving the outcome.

**Check** one actor with several colliders, two valid actors, repeated overlap, and a recycled target object. Confirm the observed hit count matches the chosen policy.

### Separate selection from validation

If assistance chooses a target, validate reach, obstruction, and action eligibility at the commit point. Drive sound, effects, and reactions from the resolved outcome so a blocked or interrupted action can communicate its actual result.

**Check** a chosen target leaving reach, an obstacle entering the path, and a target becoming unavailable during windup. Ask a player to distinguish miss, block, and hit with reduced effects, and verify recorded outcomes against the displayed feedback.
