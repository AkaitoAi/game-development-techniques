# Controller haptics and force feedback

## Problem and design reasoning

**Problem.** Players see several events at once and cannot distinguish an impact, surface, or interaction by touch because every effect feels like the same buzz.

**Design reasoning.** Team Asobi maps gameplay data into distinct signals. Its approach suggests authoring tactile cues around what the player should feel or recognize, then keeping the simplest recognizable version for devices with fewer capabilities.

Treat touch as a feedback channel with its own timing, intensity, and limits. Use it to reinforce a readable event or distinguish a physical interaction.

## Shipped examples

**Star Fox 64 / Lylat Wars (1997).** Nintendo shipped the game alongside the controller's Rumble Pak, making controller vibration part of the experience. Simple vibration remains a useful fallback when a device lacks richer tactile output. [Nintendo's retrospective on Lylat Wars](https://www.nintendo.com/de-ch/News/2011/Star-Fox-64-3D-weckt-Erinnerungen-bei-Publizist-Winnie-Forster-257908.html)

**Astro Bot (2024).** Team Asobi used haptics and adaptive triggers to distinguish surface textures, object interactions, and abilities. Its lead gameplay programmer demonstrates how gameplay data maps to tactile waveforms and how the effects are authored alongside visuals and audio. [Team Asobi, Feel the World, GDC](https://gdcvault.com/play/1035347/Feel-the-World-The-DualSense)

## Techniques and practical checks

### Author patterns around semantic events

Emit an event for contact, impact, surface change, or resistance with an intensity and context. Give each event a short, recognizable envelope. For sustained terrain effects, derive intensity from speed and blend between materials. Map the event to a richer waveform or a simple rumble pulse according to device capability.

**Check** repeatable material transitions at several movement speeds on each supported device. Ask players to distinguish intended sensations, and verify that a tactile cue begins at the same gameplay moment as its visual and audio cues.

### Mix effects by priority

Give transient high-priority cues a short period of prominence over background effects. Limit concurrent patterns and clamp amplitude after mixing. Assign trigger resistance an owner so two interactions cannot each assert a different force simultaneously.

**Check** several impacts over a sustained effect, rapid transitions between trigger modes, and simultaneous local players. Watch for saturation, swallowed cues, and effects routed to the wrong controller.

### Support intensity, comfort, and fallback

Offer adjustable intensity and an off option. Where practical, let players control trigger resistance separately from vibration. Provide another channel for cues that carry essential gameplay information. Check the input path still accepts the same action on devices without adaptive resistance.

**Check** repeat the same task at maximum, reduced, and zero intensity and on a device without haptics. Verify completion, readability and equivalent gameplay timing. Include users who choose lower intensity or use adaptive controls.

### Tie output to device and interaction lifetime

Cancel sustained effects when their source ends, changes ownership, or leaves the game. Clear actuator state on pause, focus loss, controller disconnect, and shutdown, then restore eligible effects on resume. Avoid replaying expired impulses after a device reconnects.

**Check** disconnect a device during a sustained pulse or trigger effect, switch active controllers, and pause during a scripted interaction. On recovery, confirm the old state is cleared and the right device receives the next fresh cue.
