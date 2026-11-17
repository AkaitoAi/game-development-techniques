# Motion capture pipeline

## Problem and design reasoning

Usable game animation depends on the whole path from the stage to the character in the build. A capture can preserve the performance but still fail because a prop was untracked, the solve drifts, or the exported clip starts outside the gameplay system's expected pose. Decide before the shoot which contacts, timings, and degrees of freedom the game needs. Preserve the reference video and raw data so an animator can tell a performance choice from a processing error.

**Documented.** Naughty Dog built cleanup, curve conversion, retargeting, and delivery tools to handle the volume and quality requirements of *The Last of Us*. Ninja Theory studio head Dom Matthews said *Hellblade II* used performance capture to preserve the cast's performances and support immersion. **Inference.** A studio can apply those priorities at a smaller scale by automating repeatable conversion work and retaining manual review for character intent, hands, feet, and prop contact. [Damon Shelton, GDC 2015](https://www.gdcvault.com/play/1021854/contactUs) · [Ninja Theory interview, Xbox Wire](https://news.xbox.com/en-us/2024/05/16/hellblade-2-performance-capture-next-level-combat-design/)

## Shipped examples

- **Uncharted 2: Among Thieves (2009)** combined casting, writing, capture, and downstream animation in a cinematic production process that the team adapted to its schedule and scope. Its postmortem records that the team added live audio capture on the motion capture stage. [Amy Hennig and Josh Scherr, GDC 2010](https://www.gdcvault.com/play/1012807/Behind-the-Scenes-Uncharted-2) · [Naughty Dog postmortem](https://media.gdcvault.com/GD_Mag_Archives/GDM_March_2010.pdf)
- **The Last of Us (2013)** used tools to clean raw motion data, produce animation curves, retarget onto character skeletons, and deliver the results to animators. [Damon Shelton, Capturing The Last of Us: Motion Capture Pipeline, GDC 2015](https://www.gdcvault.com/play/1021854/contactUs)
- **Senua's Saga: Hellblade II (2024)** used performance capture as the starting point for character animation. The production recorded facial and physical performance together in some shots and separately in others. [Ninja Theory launch post](https://news.xbox.com/en-us/2024/05/21/hellblade-2-launch-blog/) · [Ninja Theory interview, Xbox Wire](https://news.xbox.com/en-us/2024/05/16/hellblade-2-performance-capture-next-level-combat-design/)

## Techniques and practical checks

### Prove the shot and clip list before the session

Record what each clip must do in the game. Specify character and prop roles, contact points, playable entry and exit poses, forced timings, and which video, voice, face, or body streams must be synchronized. Capture a short representative rehearsal and run it through the actual import path.

**Check** a rehearsal with the most important prop contact, a moving actor, and a transition back to player control. Confirm the stage setup can capture the material and that the imported motion meets the intended transition and interaction.

### Preserve traceable inputs and take selection

Link raw streams, timecode or sync markers, calibration, reference video, and take notes with stable identifiers. Give a selected take an explicit status and keep a manifest of the source take, solve settings, export settings, and target rig version. Reprocess from the raw assets when processing changes.

**Check** by extracting a selected take on a clean workspace and rebuilding its intermediary. Verify body, face, and audio alignment from the beginning and end of the take. Replace one stream with a mismatched take and verify the validation exposes the mismatch.

### Review the solved data before polishing

Work in stages. Inspect gaps, discontinuities, and orientation on the capture skeleton first. Review scaling, feet, hands, root motion, and prop contacts after mapping to the game skeleton. Reserve the editable animation pass for cleanup that preserves intent and adjusts to gameplay needs.

**Check** a planted foot, hand on a fixed prop, and turn in place on the capture skeleton and the destination character. Compare contact errors in world space, root displacement, and time alignment at matched frames.

### Accept the clip through gameplay

Test the exported asset with the real timing driver, locomotion transition, and required collision or interaction setup. For a paired animation, confirm both actors and props interpret the same anchors and time. Measure the difference between the authored contact frame and the gameplay event.

**Check** the action from its supported approach offsets and speeds. Interrupt it immediately before and after the contact event, replay it at supported playback speeds, and verify that transitions, effects, and editable takes remain traceable.
