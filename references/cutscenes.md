# Cutscenes and cinematic scripting

## Problem and design reasoning

**Problem.** A scene starts from an impossible live pose, skipping leaves triggers unapplied, or a recording cannot represent player customization.

**Design reasoning.** Santa Monica describes staging for God of War's continuous camera and solving the positions needed at gameplay handoff. A design inference is to pick the cinematic format around runtime dependencies and specify the gameplay state reached on either completion or skip.

## Shipped examples

**Final Fantasy VII (1997).** Square Enix identifies the original game as the series' first to feature cinematic pre-rendered cutscenes. A pre-render gives the team control over the finished image and playback cost. If a scene needs to show a player's chosen appearance or reflect the current environment, it needs suitable variants or a way to render those differences during play. [Square Enix retrospective](https://www.square-enix-games.com/en_GB/news/final-fantasy-vii-nintendo-switch-xbox-one-release-date)

**God of War (2018).** Santa Monica Studio developed a continuous camera approach and used live-action previsualization to test staging. The team describes solving cases where characters needed to arrive at a specific position for a transition without a camera cut to conceal the move. This makes blocking and the handoff from gameplay part of the cinematic design. [Erica Pinto, GDC 2019](https://www.gdcvault.com/play/1026025/Keyframes-and-Cardboard-Props-The) · [Santa Monica Studio encounter breakdown](https://blog.playstation.com/archive/2018/08/16/santa-monica-studio-details-the-epic-creation-of-god-of-wars-unforgettable-stranger-fight/)

**Final Fantasy XVI (2023).** Square Enix presented a production pipeline covering motion-data transfer between authoring tools, voice and camera tooling, and per-shot progress tracking. Its staged sequence workflow makes dependencies visible before final capture, layout, and review. [Eitaro Iwabuchi, GDC 2024](https://www.gdcvault.com/play/1034299/Cinematic-Workflow-and-Pipeline-of)

## Techniques and practical checks

### Choose the format for the scene's dependencies

List the player choices, costume variants, world states, and accessibility layers the scene needs to reflect. Use a pre-rendered video where fixed staging and known variants meet those needs. Use real-time staging when the scene depends on active actors, equipment, or uninterrupted camera movement. An animatic can establish the cut timing for either format. Budget video decoding and storage for pre-rendered scenes, and actor, lighting, and effects costs for real-time scenes.

**Check** a representative beat on minimum hardware with the largest cast and hardest lighting setup expected. Try two different player appearances, pause and resume, and measure either decoder load or render frame time. Verify that the correct variant plays and that subtitles remain synchronized.

### Validate the handoff from gameplay

Declare entry requirements for actors, camera, controls, scene assets, and quest state. Use a staging area or an authored transition to align actors whose starting positions can vary. Transfer camera and input ownership explicitly, and return both at an exit pose that works with the gameplay camera and movement system.

**Check** the sequence from every valid approach, with obstructed paths, moving actors, and input held across both transitions. Inspect framing and motion at the handoff and verify that the player regains control without inheriting stale input.

### Keep story outcomes consistent when playback changes

Give the scene an idempotent completion path for required state changes, such as quest flags or actor placement. Specify whether skip or checkpoint recovery completes the scene, restores the entry state, or resumes at a safe point. Tie camera and audio cleanup to those exits and keep purely presentational events on the timeline.

**Check** normal playback, skip immediately before and after a story event, and reload mid-scene where supported. Compare the expected final quest state, actor placement, inventory, and input ownership across the applicable paths.

### Review timing and readability in the packaged scene

Build shot timing with temporary dialogue and blockouts before final animation. Give dialogue, camera, animation, and captions stable cue or shot identifiers. Review the scene with shipping camera limits, accessibility settings, aspect ratios, and localized timing. Preserve authored cuts where they carry meaning and update cue timing when picture edits move them.

**Check** with a longer localized line, a narrow viewing area, captions enabled, and reduced camera motion where supported. Confirm that speakers and important actions remain identifiable and that a caption stays on screen long enough to read.
