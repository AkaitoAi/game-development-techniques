# Mobile frame pacing

## Problem and design reasoning

Players can see uneven motion when frame presentations alternate between early and late even if average FPS meets its target. The shipped Android MMORPG *Mir 2: Return of the King* used the Android Frame Pacing library to improve presentation stability. Google's case study describes a reduction in slow sessions on both graphics APIs tested. Apple’s WWDC21 session explains how presentation cadence and system conditions affect the available rates on variable-refresh displays. The recommendation to prefer a predictable sustainable cadence for each device tier is an inference from those sources. [Mir 2 developer case study](https://developer.android.com/stories/games/swappy) · [Apple, Optimize for variable refresh rate displays, WWDC21](https://developer.apple.com/videos/play/wwdc2021/10147/)

## Techniques and verification

### Measure actual presentation intervals

Capture CPU and GPU work alongside display presentation timestamps where the platform allows it. Look at a histogram of elapsed time between presented frames, missed presentation deadlines, and the longest run of late intervals. A steady camera pan makes cadence problems visible at constant motion. [Android Frame Pacing library](https://developer.android.com/games/sdk/frame-pacing)

**Check** — Replay a steady pan and a busy encounter on a fixed-refresh phone. Record median and 95th-percentile presentation intervals and count intervals that exceed the intended cadence. Inspect the same capture for CPU stalls, GPU stalls, and missed compositor deadlines.

### Choose a cadence the current display can sustain

On fixed refresh, prefer a supported display mode and an integer presentation divisor when possible. For example, 30 FPS fits a 60 Hz display, and 40 FPS fits 120 Hz. On variable refresh, request a rate or range through the supported presentation mechanism and observe the actual mode the system provides.

**Check** — Run a known test scene at each offered cap on a 60 Hz and a high-refresh device. Change OS power mode and watch the selected rate and presentation histogram. Confirm the game handles a rejected or changed display mode without oscillating between caps.

### Limit queue depth while keeping the pipeline fed

Use a supported frame pacer or engine integration where available. Account for CPU submission, GPU execution, and presentation. Overfilling the queue can preserve throughput while adding latency. Keep rate changes separate from gameplay simulation and sample input on a timeline consistent with the game's simulation model. [Android Frame Pacing library](https://developer.android.com/games/sdk/frame-pacing)

**Check** — Compare a steady test route and a short GPU workload burst before and after changing the pacing integration. Check frame-interval distribution and input-to-visible-response latency. Verify both results improve or meet their documented targets.

### Reset presentation timing after discontinuities

Re-evaluate the presentation target after backgrounding, switching refresh mode, or recreating a render surface. Treat the gap as a lifecycle transition so stale timing and accumulated rendering work cannot trigger a visible burst when returning.

**Check** — Repeat background and foreground cycles, switch display refresh modes, and rotate where supported. Measure the time to first stable presentation, then confirm frame deadlines settle and game simulation proceeds at the expected rate.
