# Platform porting

## Problem and design reasoning

**Problem.** The same game misbehaves on a different display, device, or lifecycle, even though the original platform remains stable.

**Design reasoning.** Doom's platform seam isolates window input and display behavior from common play. Nixxes and Guerrilla's hardware tiers suggest testing representative capability groups and keeping the game outcomes comparable across presets.

## Shipped examples

**Doom on Linux.** In id Software's released Linux source, the main game logic and the X11 display and input implementation live in separate source files. The platform layer translates X11 events into game input events. This seam remains useful when adding a new window system or platform input backend. [Game loop](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/g_game.c) · [X11 platform implementation](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/i_video.c)

**Horizon Forbidden West Complete Edition on PC (2024).** Nixxes and Guerrilla published hardware tiers paired with graphics presets, resolution, and frame-rate targets. The release also supports unlocked frame rates and ultrawide displays. These features expand the test space beyond a single console configuration. [Nixxes hardware targets](https://blog.playstation.com/2024/03/05/horizon-forbidden-west-complete-edition-pc-specifications-revealed-out-march-21/) · [Guerrilla and Nixxes launch notes](https://blog.playstation.com/2024/03/21/horizon-forbidden-west-complete-edition-lands-on-pc-today/)

## Techniques and practical checks

### Establish the platform boundary

List each platform-dependent operation, including input, windowing, saves, user identity, and lifecycle. Define the behavior game code expects from each seam. Keep platform error codes and asynchronous completion details in the adapter. Return enough information for gameplay to distinguish retryable failure from an unavailable feature.

**Check** the same gameplay flow with each platform backend. Revoke access to storage, disconnect a device, and lose or regain focus with an action held. Check both the visible outcome and the state of pending operations.

### Run a packaged vertical slice early

Choose one scene with representative graphics, input, persistence, and a loading transition. Package and install it on target hardware early enough to expose compiler, asset conversion, and runtime dependencies. Track where target hardware requires a capability fallback.

**Check** the package on a clean device or user profile. Cold-launch, enter gameplay, save, exit, relaunch, and resume. Repeat with missing optional hardware and with high memory pressure.

### Build a test matrix around capabilities

Group targets by meaningful constraints such as memory class, CPU and GPU budgets, display geometry, input device, and lifecycle. Define baseline goals in milliseconds and memory units. Combine the most-used settings with cases where two constraints interact, such as ultrawide rendering while using a low-memory preset.

**Check** representative traversal and dense gameplay on the lowest supported tier. Compare frame-time tails, memory peaks, and readability. Confirm that the same actions behave as intended at both capped and uncapped render rates.

### Verify differences at the point of use

Keep a fixture with known visual landmarks, timing windows, and save state. Compare outcomes across builds with tolerances appropriate to the feature. Classify intentional platform differences so a test can distinguish a permitted visual change from a gameplay regression.

**Check** changing quality, aspect ratio, and input device during actual play. Verify HUD bounds, button prompts, gameplay timing, and save recovery after suspend or reset.
