# Sports game systems

## Problem and design reasoning

Sports games need responsive actions, recognizable rules, and enough time for tactical decisions. Symptoms include a convincing animation that makes a shot late, a stoppage that players cannot explain, or a small-sided variant in which everyone crowds the ball. EA documented that the Rush pitch proportions and offside adaptation in *FC 25* were intended to encourage lateral passing and reduce congestion and stoppages. A design inference is that a variant should tune space, time, and rules together because changing one alters the value of the others. [EA Sports FC 25 Rush Deep Dive](https://www.ea.com/games/ea-sports-fc/fc-25/news/pitch-notes-fc-25-rush-deep-dive)

## Shipped examples

- **Wii Sports (2006).** Nintendo's interview describes varied ball motion from the remote swing and a package of five sports that playtesters of different ages could enjoy. The transferable lesson is to preserve a recognizable sporting decision, such as timing or directing the ball, through the input the player can perform comfortably. [Nintendo developer interview](https://iwataasks.nintendo.com/interviews/wii/wii_sports/0/3/)
- **FIFA 22 (2021).** EA captured a full match with professional teams to address limitations of conventional studio capture. Its GDC presentation connects that data to athletic animation, ball physics, and its ML Flow animation system. [EA, GDC 2022 Animation Summit](https://gdcvault.com/play/1027595/Animation-Summit-FIFA-22-s)
- **EA Sports FC 25 (2024).** Rush uses a smaller pitch and applies offside in the attacking third. Its buzzer-beater rule lets a shot already in flight resolve after time expires. This is a useful example of preserving the stakes of a final action while changing match length. [EA Sports FC 25 Rush Deep Dive](https://www.ea.com/games/ea-sports-fc/fc-25/news/pitch-notes-fc-25-rush-deep-dive)

## Techniques and practical checks

### Model the match as explicit rule states

Declare play, stoppage, restart, tiebreak, and result states with the allowed transitions and clock behavior. Define the priority of same-tick events and publish the ruling as a structured event so commentary, UI, and replay can use it.

**Check** a score arriving on the same tick as a boundary violation or clock expiry. Repeat the input trace and compare the ruling, clock, score, and restart possession.

### Align player intent, animation, and contact

Record the target, intended direction, and action window when an input is accepted. Define when the rules commit a ball strike, catch, or possession change. Feed the resolved outcome to animation and audiovisual feedback. Preserve legal interruption during windup.

**Check** actions at shallow angles, under repeated inputs, and during a sharp change in movement. Measure input-to-response time, contact alignment, and the frequency of outcomes that contradict the preview.

### Teach tactics through space and team behavior

Tune playable area, player count, and restart placement together. Give off-ball players roles with coverage constraints and predictable role changes when possession turns over. Show nearby passing options or space through field markings and camera framing where appropriate.

**Check** the same small-sided fixture with novice, practiced, and mixed-skill teams. Track off-ball involvement, width of play, interruptions per minute, and abandoned possession options before tuning pitch size or rules.

### Compare assistance across ability levels

Expose meaningful control over supported actions and assistance, and define which assists are legal in each mode. Track aim, timing, and movement separately when deciding what the assistance should correct.

**Check** identical practice drills with and without assistance. Compare successful actions, false corrections, and whether input intent is still legible in replay. Confirm competitive mode applies the published assistance rules.
