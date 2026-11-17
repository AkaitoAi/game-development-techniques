# Game feel

## Problem and design reasoning

**Problem.** A near-correct input is lost on a boundary, movement feels slippery, or impacts register in simulation without a perceptible result.

**Design reasoning.** Celeste's published code shows buffered inputs and grace windows. A design inference is to treat intent as a short-lived signal and make the outcome perceptible, while measuring delay at input, simulation, and presentation before tuning.

Game feel is the moment-to-moment relationship between what the player intends, what the controls register, how the simulation responds, and what the player perceives. Tune those parts together by repeating the same action in a small test space and in representative gameplay.

## How shipped games shape feel

- **Super Mario 64 (1996)** maps analog stick direction to movement direction and stick tilt to speed. The player can use small movements to line up a jump and wider movements to run. Nintendo's manual describes Lakitu camera controls alongside character controls because the viewpoint is part of playing a 3D space. [Nintendo's original manual](https://www.nintendo.com/eu/media/downloads/games_8/emanuals/nintendo_8/Manual_Nintendo64_SuperMario64_EN.pdf)
- **Celeste (2018)** builds forgiveness directly into movement. The published player code tracks ground-jump grace after leaving a ledge, consumes buffered jump input on execution, and includes adjustments for upward corner collisions. These choices extend the opportunities to carry out a precise intended move. [Published Celeste player code](https://github.com/NoelFB/Celeste/blob/master/Source/Player/Player.cs)
- **Street Fighter 6 (2023)** offers Classic and Modern input schemes. With Modern controls, a player can perform supported special moves using simpler directional and button inputs. This is an example of offering another way to express an action at the command-recognition layer. [Capcom's controls and battle system overview](https://news.capcomusa.com/2022/06/02/street-fighter-6-redefines-the-genre-in-2023/)

The examples illustrate different layers of the same input-to-response chain across hardware and generations. Analog movement opened a wider range of responses to physical input. Modern precision platformers can shape the simulation's interpretation of near misses, while an action game with several command schemes can let players choose how they express a move. Vlambeer's 2013 workshop adds the presentation layer with before-and-after demonstrations of effects such as camera shake. [Jan Willem Nijman's The Art of Screenshake](https://www.youtube.com/watch?v=AJdEqssNZ-U)

## Techniques

### Tune acceleration and stopping together

Choose target speed, time to reach it, stopping distance, and reversal time. For analog input, measure stick drift and test a dead zone and response curve that leave room for slow movement. Give designers access to parameters in a repeatable test space. **Check:** Use marked lanes for short taps, full runs, and reversals. Compare stopping points across input devices and supported frame rates, then repeat on slopes and moving surfaces.

### Buffer input and allow a small grace period

Record a fresh press with its time and consume it once when the action becomes legal. For a platformer, test a short pre-landing jump buffer and a short post-ledge grace period independently. Specify which transitions consume or clear each opportunity. **Check:** Replay presses immediately before and after landing or departure, exactly at expiry, and after interruption by respawn or knockback. Verify that holding the button only repeats the move if repetition is intended.

### Make impact feedback identify the result

Emit a confirmed contact event carrying the point, direction, and outcome of the collision. Drive a short sound, animation accent, or particle burst from that event. If adding hit stop or camera impulse, start small, define which clocks keep running during hit stop, and provide reduced-motion controls where relevant. **Check:** Ask a player to identify a miss, a block, and a hit during a crowded encounter, then repeat with audio muted and camera effects reduced. Watch for input loss during hit stop and runaway feedback when impacts stack.

### Measure responsiveness end to end

Log the first observed input, the simulation tick that accepts it, and the first visible response. Use a high-frame-rate recording of the controller and display when you need to include device, rendering, and display delay. **Check:** Compare typical and slow-tail response times on target hardware, including under load. Distinguish intentional wind-up from a late acknowledgment by checking how soon the action becomes visible or audible.
