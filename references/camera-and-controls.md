# Camera and controls

## Problem and design reasoning

**Problem.** The desired camera pose crosses geometry, automatic rotation competes with manual input, or players lose orientation during mode changes.

**Design reasoning.** Nintendo documents separate character and camera input. Ragnarök documents configurable camera assistance. A design inference is to give viewpoint control explicit ownership and a recovery action even when the default follow behavior is automatic.

## Shipped examples

**Super Mario 64 (1996).** The controller separates analog character movement from camera commands on the C buttons. The manual presents Lakitu as a camera operator and describes changing the view and zoom during play. This establishes an explicit way for the player to recover a useful viewpoint in a 3D space. [Nintendo instruction manual](https://www.nintendo.com/eu/media/downloads/games_8/emanuals/nintendo_8/Manual_Nintendo64_SuperMario64_EN.pdf)

**God of War Ragnarök (2022).** Santa Monica Studio exposes automatic camera and targeting behavior as settings, including recentering on attacks and navigation assistance that orients the view toward the next story goal. Motion settings include camera shake adjustment and a persistent center dot. The documented settings give players ways to balance manual control, orientation, and comfort. [PlayStation accessibility reference](https://www.playstation.com/en-us/games/god-of-war-ragnarok/accessibility/) · [Santa Monica Studio accessibility overview](https://sms.playstation.com/stories/god-of-war-ragnarok-accessibility-features)

**What changed.** Super Mario 64 uses discrete camera commands alongside continuous analog movement. Ragnarök demonstrates how several forms of camera assistance can be configured independently. Both approaches support an explicit way to regain orientation. Discrete camera commands remain useful for limited input devices and games that rely on predictable composition.

## Techniques to try

### Map each device to gameplay actions

Normalize bindings into move, look, recenter, aim, and lock-on actions. Treat mouse motion as a per-sample displacement and stick deflection as an angular rate integrated over elapsed time. Tune the stick dead zone and response curve separately. For camera-relative ground movement, construct an orthonormal basis in the movement plane from camera heading, including a fallback for near-vertical views. Preserve the player's chosen bindings through context and device switches.

**Check** with stick drift, diagonals, a full stick sweep, and identical recorded mouse displacement at several frame rates. Hold a direction through a camera transition and test whether the resulting turn matches the intended control behavior. Test remapping, inversion, device switching, and focus loss.

### Separate framing from collision

Compute a desired framing pose from player position, heading, and an optional look-ahead target. Shape-cast from its pivot toward the desired camera position and pull the camera in to preserve a clearance margin. Recover toward the desired distance with damping once the path clears, then constrain the final pose against obstacles. Use a dedicated collision filter and handle teleports as explicit resets.

**Check** with doorways, thin posts, convex and concave corners, low ceilings, and obstructing props. Orbit while moving through each fixture. Record maximum time the player is occluded, any geometry intersections, and how quickly the camera recovers after clearing the obstruction.

### Give camera modes explicit ownership rules

Define entry and exit conditions for free follow, manual orbit, aim, lock-on, and scripted views as applicable. Specify which inputs interrupt each mode and when assistance resumes. Use hysteresis for target selection and lost visibility. Make recentering and automatic rotation configurable when they compete with manual look input.

**Check** by entering aim during automatic rotation, losing a target behind a wall, rapidly switching targets, and releasing aim while moving. Log which mode owns rotation, angular velocity, and target changes. After cancellation, the next manual look input should have the expected effect.

### Verify framing and comfort together

Choose how the game maintains its gameplay framing across supported aspect ratios. Separate camera effects such as shake and roll from the aim reference used to resolve player actions whenever they are presentation-only. Offer controls for strong camera effects where they are used.

**Check** on narrow and ultrawide views with shake at full strength and disabled. Confirm the visibility of targets, interactables, and navigation cues. Repeat rapid turns and representative combat or traversal with playtesters sensitive to camera motion.
