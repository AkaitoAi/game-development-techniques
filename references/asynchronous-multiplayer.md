# Asynchronous multiplayer

## Problem and design reasoning

**Problem.** Players cannot schedule a live match, but they still want competition or cooperation without waiting for the other person to connect.

**Design reasoning.** Kojima Productions describes the Social Strand System as a way to connect geographically separated players. A design inference from strands and Mario Kart ghosts is to preserve the contribution that matters, such as a route aid or recorded performance, and replay or surface it later.

Use this guide when one player's contribution should matter to another player who plays at a different time.

## Shipped examples

**Mario Kart 8 (2014).** Nintendo's manual documents racing against recorded ghosts in Time Trials. The player can compare lines and timing against another performance without arranging a match. [Nintendo, Mario Kart 8 electronic manual](https://www.nintendo.com/eu/media/downloads/games_8/emanuals/wii_u_6/mario_kart_8/ElectronicManual_WiiU_MarioKart8_EN.pdf)

**Death Stranding (2019).** In the Tokyo Game Show 2019 gameplay session, Kojima Productions demonstrated the Social Strand System. A studio campaign later documented that players can find and use items and structures left by other porters. This supports cooperation through persistent contributions in a player's own session. [PlayStation's Tokyo Game Show 2019 stage report](https://blog.ja.playstation.com/?p=523) · [Kojima Productions, Revitalize the Eastern Region](https://www.kojimaproductions.jp/index.php/en/Revitalize_the_Eastern_Region_2024)

**Death Stranding 2: On the Beach (2025).** Kojima Productions retained the Social Strand System in the sequel. Its launch interview describes it as a way for geographically separated players to connect. This is a useful example of a persistent social system carrying across titles with different traversal and world design. [Kojima Productions launch interview](https://blog.playstation.com/2025/06/25/death-stranding-2-on-the-beach-launch-interview/)

## Techniques and practical checks

### Define the contribution and how recipients discover it

Pick a small meaningful record such as a ghost run, turn, map marker, or shared structure. Include a stable identifier, target location or ruleset, and version. Decide which recipients may receive it and when it should appear. Cap or rotate records per location so a busy area remains readable and new contributions have a chance to surface.

**Check** by submitting records with two accounts and visiting the same area at different progression stages and activity levels. Verify that eligible contributions appear, irrelevant ones stay hidden, and navigation stays readable at the density cap.

### Make delayed writes retryable

Assign each submission an operation identifier and keep the receipt when it succeeds. Use authoritative revisions to resolve competing turns or edits. Specify what the client shows while offline, pending, accepted, or rejected, and handle records that expire while the client is disconnected.

**Check** by losing the acknowledgement after a successful submission, retrying following reconnect, and returning from a long absence. Verify that one item, reward, or turn is committed and that the client shows a clear status for retired records.

### Keep records compatible with the receiving world

Validate records against the track, map, objectives, and ruleset. Associate ghosts with compatible content versions. For functional world objects, check placement, collision, progression gates, and resource capacity before activation. Give the player a safe fallback if a record becomes invalid after an update.

**Check** by loading a record after changing a track layout, locking a shortcut, and moving a terrain boundary. Verify that obsolete records are withheld or migrated and the base route remains playable when the service is unavailable.

### Give players control over shared presence

Show whether contributions are public, friend-scoped, or private. Offer controls to hide contributions or report inappropriate ones. Prefer structured content where it meets the design need. If custom labels or messages are allowed, apply review and visibility rules before distribution.

**Check** with a private profile, two blocked accounts, and a removed record. Verify that fresh requests and cached views honor those choices and that core gameplay remains available with contributions disabled.
