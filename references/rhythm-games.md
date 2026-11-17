# Rhythm games

## Problem and design reasoning

**Problem.** Players tap consistently early or late, cannot read a chart at speed, or see gameplay and music fall out of sync. Distinguish predictable latency from unstable timing and unclear cues.

**Design reasoning.** Tango describes a shared synchronization system and beat-based rewards. A design inference is to share a time reference while setting timing strictness per activity, since a bonus and a required input have different consequences. See the shipped examples below for sources.

## Shipped examples

**Rock Band (2007).** Harmonix's postmortem explains that its familiar 3D note lanes communicated the drum pads more clearly than the kick pedal in a prototype. The physical position and gesture of an input affect whether a cue is easy to interpret. [Harmonix, Game Developer postmortem, May 2008](https://media.gdcvault.com/GD_Mag_Archives/GDM_May_2008.pdf)

**Hi-Fi RUSH (2023).** Tango Gameworks describes a shared music synchronization system used by gameplay and sound, and a rhythm-action design that rewards playing on the beat. Start by choosing which actions require precise timing and which ones merely benefit from it. [Tango Gameworks, CEDEC 2023 music and game production session](https://cedec.cesa.or.jp/2023/session/detail/s6423d5c393c96.html) · [John Johanas, GDC 2024 design talk](https://gdcvault.com/play/1034256/Developing-Hi-Fi-RUSH-Backwards)

## Techniques and practical checks

### Put chart and scoring on one timeline

Choose an audio presentation clock or a monotonic timeline mapped to it, then express note positions in beats and convert them through a tempo map. Derive cue placement and input judging from the same mapping. Preserve an explicit timeline offset for lead-in silence and define pause, seek, and loop behavior.

**Check** a chart with a lead-in, a tempo change, and a loop. Compare scheduled note times with visible cue arrivals and the recorded audio. Pause or seek near the tempo change and repeat under uneven frame pacing to check for drift.

### Calibrate the playback and input paths

Capture input timestamps as close to the device event as possible. Treat visual synchronization and judgement calibration as separate adjustments when the game can expose them clearly. Use a repeatable calibration phrase and explain how players can reset or repeat the test if their display or audio path changes.

**Check** the same chart with wired and wireless audio and gamepad input. Collect signed early and late errors before and after calibration. Confirm that removing the offset reverses the adjustment and changing frame rate does not systematically shift scores.

### Design readable charts and timing feedback

Show enough upcoming notes to plan the required gesture. Tie each cue's spatial arrangement to how the player performs it. Define what happens when several notes are eligible within the same timing window. Provide early and late feedback and use distinct shapes, movement, or other redundant cues for important differences.

**Check** simultaneous notes, closely spaced repeats, and mixed pad and pedal gestures with new and experienced players. Observe which device actions they misread and repeat with audio muted to assess the visual cues.

### Make advanced accuracy rewarding

For rhythm action, choose whether music timing gates actions, strengthens them, or affects presentation and score. Document the effect of each timing grade. Give beginners room to learn the rhythm while preserving distinctions that matter for advanced play.

**Check** one encounter with on-beat, consistently late, and unpaced inputs. Compare progress, score, and displayed feedback against the design target, then repeat with the available timing assists to verify their intended effects.
