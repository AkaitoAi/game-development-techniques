# Mobile device coverage and capability tiers

## Problem and design reasoning

One Android or iOS build can run on devices that differ in graphics capability, memory, display geometry, operating system, and driver behavior. A successful launch on the development phone covers only one combination. Set a baseline experience and define the evidence needed to enable each optional feature. Group tests by shared failure modes and keep real hardware for the cases it can reveal.

## Shipped examples

- **Broken Age.** Double Fine's GDC 2014 talk *Scaling from Mobile to High-End PCs: The Tech of Broken Age* describes targeting mobile and desktop with different GPU features and performance characteristics. The presentation identifies limited knowledge of available Android device capabilities as a problem. Double Fine's support page documents both the historical mobile release and the eventual end of current Android support. The transferable inference is that support commitments should have a tested boundary and a plan for changes over time. [Developer's GDC slides](https://www.p1xelcoder.com/resources/GDC_2014_Scaling_from_Mobile_to_High-End_PCs_The_Tech_of_Broken_Age.pdf) · [Double Fine support](https://www.doublefine.com/games/support/broken-age/system-requirements?platform=all)
- **Fortnite on Android.** At Khronos Dev Day during GDC 2019, Epic and Samsung discussed enabling the Vulkan rendering path for hardware and OS versions able to use it, alongside OpenGL ES. That historical deployment illustrates how graphics API choice can vary within one platform. The inference is that a capability policy should be verified against real drivers and workload measurements before promotion. [Samsung's GDC 2019 report with presentation link](https://developer.samsung.com/sdp/blog/en/2019/03/27/gdc19-post-game-report)
- **Minecraft Bedrock Edition, Vibrant Visuals.** Mojang documents a hardware floor for its optional renderer on mobile and a way to choose a graphics mode. The feature adds visuals without changing gameplay, which lets cohorts on different render paths share worlds. [Mojang feature guide](https://www.minecraft.net/en-us/vibrant-visuals-update)

## Techniques and practical checks

### Build a representative matrix

Select phones and tablets around the oldest supported OS, lowest supported memory, common GPU families, unusual screen geometry, and newer refresh modes. Include popular configurations and combinations near support thresholds. Assign a reason, test route, and acceptance criteria to each case.

**Check** the packaged build on at least one physical device in each selected high-risk group. Log the model, OS, graphics backend, configured tier, build ID, and result so the matrix can explain coverage gaps.

### Gate features and keep a recoverable baseline

Detect required capabilities for each optional rendering or content feature, and use validated defaults for combinations you support. If an optional startup path fails, recover through a known compatible mode with a clear setting for the player. Keep profile data maintainable by documenting the reason and review conditions for device-specific rules.

**Check** the boundary on both sides of each gate, including an unrecognized device profile and a simulated initialization failure. Verify the fallback opens a previously created save and displays all required gameplay cues.

### Test mobile transitions on real hardware

Run through cold launch, returning from task switching, interruption by system overlays, orientation changes where supported, and memory pressure. Include displays with cutouts, tablets, and unusual aspect ratios. Use simulation for rapid layout checks and physical devices for real GPU, thermal, memory, and lifecycle behavior.

**Check** a touch-driven sequence during an interruption, then confirm the game clears stale input, restores a playable viewport, and resumes or recovers progress according to the design.

### Maintain support with observable release gates

Track startup success, crashes, rendering faults, low-memory exits, and frame-time tails by useful device cohorts. Keep measurements proportional to the question and avoid collecting device identifiers where aggregate capability classes are sufficient. Review the matrix after an engine upgrade, OS update, or material change in device mix.

**Check** that the canary or pre-release report can distinguish a regression specific to one graphics backend from a broad release failure. Confirm that the team can keep the baseline available while investigating an optional path.
