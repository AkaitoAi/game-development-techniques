# Camera composition and photo mode

## Problem and design reasoning

A camera can track the player correctly while still losing an important subject, cutting at a confusing moment, or cropping the action at the edges of a supported aspect ratio. Photo mode introduces a different requirement. Players want to reframe a moment and change its presentation while returning to the same playable state afterward. Treat screen composition as an authoring goal and give photo tools their own temporary settings and simulation policy.

## Shipped examples

**God of War (2018).** Santa Monica Studio's cinematography talk presents the no-cut camera as one of the tools the team employed to build a deeper emotional connection. An implementation inference is that continuous framing can make moment-to-moment camera transitions feel like part of the staging, provided the team plans the handoff between authored and player-controlled views. [Santa Monica Studio, GDC 2019](https://www.gdcvault.com/play/1025986/Creating-a-Deeper-Emotional-Connection)

**Gran Turismo Sport (2017).** Scapes lets players place their cars into photographed locations. PlayStation describes the approach as an HDR workflow that preserves scene light information for physically based rendering. The documented goal was to make a personalized car look like it belongs in a real location. [PlayStation feature breakdown](https://blog.playstation.com/2017/09/14/gran-turismo-sport-eight-things-you-need-to-know/)

**Ghost of Yōtei (2025).** Its photo mode offers focal length, focus distance, aspect ratio, environmental adjustments, and multiple positions for an animated camera path. PlayStation's examples freeze characters while keeping wind-driven environment motion active to produce looping or moving images. An implementation inference is that independent presentation controls make a useful alternative to an all-or-nothing simulation freeze. [PlayStation photo mode feature guide](https://blog.playstation.com/2025/09/24/ghost-of-yotei-photo-mode-features-detailed-tips-guide/)

## Techniques and practical checks

### Compose around the important subjects

Choose the subject anchors, visible screen region, horizon intent, and optional foreground elements for a shot. Compare the subject's projected bounds with the intended composition before adjusting the camera. Change camera position for perspective and lens angle for framing.

**Check** the same dialogue, arrival, or vista shot on narrow, standard, and ultrawide displays. Confirm that relevant faces and gestures remain visible, including when subtitles occupy their largest supported area.

### Author camera transitions with measurable limits

Record the expected outgoing and incoming poses, lens settings, and subject anchors. Let an authored route interpolate position, orientation, and lens parameters explicitly. Sample between keys for obstacle clearance and sudden changes in motion. Define when the player's input can interrupt the move.

**Check** traverse each key pair forward and after a restart. Inspect intermediate frames near doorways and walls and confirm that regaining camera control continues from a compatible pose.

### Isolate photo mode state

Give photo mode a separate camera and temporary overrides for exposure, grades, time of day, visibility, and environment motion where supported. Define which systems pause, which continue visually, and which actions can enter or leave the mode. Restore the prior gameplay view and state on every exit.

**Check** enter during idle and motion, edit lens and environment settings, capture, and exit repeatedly. Compare player state, camera, time of day, particles, and pending inputs with their pre-entry values or the documented resume policy.

### Test capture and playback separately

Offer a clean capture path that excludes authoring controls. Use consistent saved aspect ratio and exposure, and choose how the capture path handles temporal history at its first frame. Keep interactive preview performance within the project's budget while exposing higher-quality capture where supported.

**Check** capture the same scene as a still and a camera move. Inspect first-frame blur, edge crop, overlays, color on the target display, and whether playback speed and framing match the preview.
