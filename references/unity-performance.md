# Unity performance on mobile, console, and PC

## Problem and design reasoning

An Editor run can hide target-device bottlenecks, and two scenes with the same average FPS can have very different hitching. Unity's optimization guides use profiling and frame inspection to locate the expensive work before changing assets or rendering settings. Choose a reproducible route and collect timing and memory on the device that has the problem. [U4, PDF pp. 54–58](unity-books.md#u4) · [U8, PDF pp. 29–36](unity-books.md#u8)

## Techniques and practical checks

- **Identify the limiting work.** Capture CPU and GPU timing where supported, then use the Frame Debugger to inspect what draws. **Check** the same camera route at baseline and reduced render resolution. Compare timings and confirm the hypothesized pass or CPU marker responds to a targeted change. [U4, PDF pp. 54–58](unity-books.md#u4)
- **Validate texture import on target.** Review per-platform compression, maximum resolution, mip requirements, and readability. **Check** memory and asset appearance before and after a texture override on the lowest supported device. Include UI text, alpha edges, and distant movement. [U8, PDF pp. 25–26](unity-books.md#u8)
- **Compare quality settings by device tier.** Treat recommendations from the older community mobile note as hypotheses for compatible projects. Preserve the authored color workflow unless visual and performance testing justify a change. **Check** alternate settings with the same lighting and materials, then compare frame time, visual output, and sustained performance after warmup. [U7, PDF pp. 1–2](unity-books.md#u7) · [U8, PDF pp. 34–39](unity-books.md#u8)
- **Test residency and repetition.** Mark when a level or feature begins loading assets and when references are released. **Check** several entry and exit cycles, compare memory snapshots at equivalent points, and check for retained textures and recurring frame spikes. [U4, PDF pp. 32–38](unity-books.md#u4) · [U9, PDF pp. 58–59](unity-books.md#u9)
