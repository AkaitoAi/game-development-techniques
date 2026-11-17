# Mobile thermal and battery budgets

## Problem and design reasoning

When a mobile build runs well immediately after launch and develops persistent stutter several minutes later, heat and sustained power draw can be limiting the device. A lower-cost feature mix or a stable frame-rate cap can leave more headroom and improve the worst part of the session. Choose what to reduce according to the workload and preserve gameplay cues. Treat changes made in response to thermal state as a device policy that needs its own verification.

## Shipped examples

- **Call of Duty: Mobile.** Samsung's engineering case study describes lowering the target frame rate when temperature rose rapidly to limit peak temperature and improve sustained stability. It discusses adjusting render quality and other workload controls, too. The Reboot Develop Red 2019 presentation *Mobilizing Call of Duty* provides conference context for the mobile optimization effort. [Samsung engineering case study](https://developer.samsung.com/galaxy-gamedev/gamedev-blog/cod.html) · [Reboot Develop Red slides](https://www.khronos.org/assets/uploads/developers/library/2019-reboot-develop-red/Mobilizing-Call-of-Duty_Oct19.pdf)
- **Minecraft Bedrock Edition, Vibrant Visuals.** AJ Fairfield's *Modernizing the Rendering of Minecraft* GDC 2026 presentation identifies resolution and thermal throttling as concerns for mobile deferred rendering and recommends soak tests both on and off external power. Mojang documents that the shipped graphics upgrade leaves gameplay rules intact and offers visual settings. A transferable inference is to keep graphics reductions independent of the simulation. [GDC slides, performance targets](https://media.gdcvault.com/gdc2026/Slides/Fairfield_AJ_ModernizingTheRenderingOfMinecraft.pdf) · [Mojang's feature and compatibility guide](https://www.minecraft.net/en-us/vibrant-visuals-update)

## Techniques and practical checks

### Compare sustained and cold runs

Replay a representative demanding route under controlled brightness, ambient temperature, refresh rate, network, and starting charge. Record frame-time percentiles, missed presentation deadlines, thermal status when available, and charge change over a session long enough to observe throttling or a stable plateau. Compare charging and unplugged runs separately. Use measured energy per minute where the hardware provides reliable power measurements and treat battery percentage as a coarse proxy.

**Check** whether a headset-free, unplugged baseline with the same route and settings shows stable performance after warm-up. Repeat with a candidate cap and compare late-session frame pacing and energy per minute at matched conditions.

### Adapt before the device falls behind

When signal support is available, use thermal status or headroom alongside frame timings to choose among reversible workload tiers. Use distinct escalation and recovery thresholds and a minimum dwell time. Reduce GPU work such as render resolution for a GPU limit and reduce nonessential simulation or UI update cadence for CPU work. Prefer display-compatible caps and keep fixed simulation steps and input sampling independent of presentation.

**Check** by injecting normal, rising, severe, and recovery thermal states through a test adapter. Verify that tiers change as expected, avoid cycling near thresholds, and preserve simulation outcomes at each frame cap. Repeat on physical hardware under sustained load. [Android Thermal API guidance](https://developer.android.com/games/optimize/adpf/thermal)

### Set workload policies by play state

Use an explicit budget for gameplay, lobby, pause, and background. Lower visual and CPU work in idle screens, stop unnecessary audio and polling when backgrounded, and retain operations needed for session or save integrity according to platform lifecycle rules.

**Check** by alternating active play, pause, and background activity while logging rendered frames, work wakeups, and network requests. Resume during a transition and verify saved state and input recover correctly.

### Protect gameplay and readability during scaling

Choose a safe order for lowering quality. Reduce cosmetic effects before changing settings that affect the clarity of an ally, hazard, or objective. Keep touch targets independent of render resolution and respect any explicit player limits.

**Check** the lowest tier in a scene dense with effects. Verify targeting, navigation signage, UI legibility, and consistent input response. On unsupported devices, test that missing thermal data still yields a bounded default and stable behavior.
