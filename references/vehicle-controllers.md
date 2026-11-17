# Vehicle controllers

## Problem and design reasoning

**Problem.** Players cannot hold the expected line, steering changes unpredictably on different surfaces, or visual wheel placement obscures the simulation. Compare response under identical inputs and separate handling from presentation.

**Design reasoning.** Suzuki describes OutRun as a driving fantasy, while Psyonix documents a fixed simulation rate and shared collision presets. The inferred principle is to choose fidelity and tuning around the driving tasks the game asks of players. See the shipped examples below for sources.

Choose the model from the driving tasks players need to perform. Speed control, drift, landing, wheel loading, and interaction with movable objects can have different fidelity requirements. Keep input interpretation, chassis simulation, and visual wheel motion independently inspectable.

## Shipped examples

**OutRun (1986).** Designer Yu Suzuki describes the game as a leisurely driving fantasy with a companion, music, and the player pulling ahead of traffic. The interview establishes a handling and presentation goal without specifying its internal vehicle equations. [Yu Suzuki interview in WIRED](https://www.wired.com/story/out-run-video-game-design/)

**Rocket League (2015).** Psyonix's Jared Cone describes a fixed 120 Hz physics update, lateral friction tuned by how much the car slides and the ground normal, and shared physics presets that define collision boxes and wheel positions independently of visual art. These choices support sharp steering and let the team iterate across multiple car bodies. [Jared Cone, Rocket League physics and networking](https://ubm-twvideo01.s3.amazonaws.com/o1/vault/gdc2018/presentations/Cone_Jared_It_Is_Rocket.pdf)

**Gran Turismo 7 (2022).** Polyphony Digital's 2024 update revised the suspension and tyre models, with changes to suspension geometry, weight transfer, and tyre heat and wear. These details matter in a game that asks players to distinguish car setups, surfaces, and repeated laps. [Polyphony Digital, update 1.49](https://www.gran-turismo.com/nz/news/00_5537934.html)

The development approaches serve different player goals. A direct yaw response can keep an arcade car readable, an arena vehicle benefits from deliberate assists around recovery and air control, and a simulator can expose weight transfer and grip behavior as part of the skill the player develops.

## Techniques and practical checks

### Build a low-complexity handling model first

For an arcade vehicle, map normalized throttle and brake into longitudinal acceleration and steer into a speed-dependent yaw target. Limit lateral slip or damp it toward a desired grip curve. Cast or sweep the chassis so walls and props constrain motion. Specify reverse, handbrake, and traction assist rules explicitly. Treat steering wheel input, stick input, and digital buttons as different signal shapes that share the same command fields.

**Check** at standstill, during a slalom, and in tight and wide turns at several entry speeds. Measure stopping distance, turn radius, and recovery from a steering reversal. Profile a pack of vehicles as well as a single car.

### Compute suspension and grip per contact when needed

For wheel-based motion, sample support under each wheel or use actual wheel contacts. Compute a spring force from compression and damping from compression speed, clamp the suspension travel, and apply forces at the appropriate chassis points to capture torque. Limit combined longitudinal and lateral forces using an approximation based on supported normal load and surface properties. Separate simulation contacts from visual wheel poses.

**Check** with a curb, a split-grip braking lane, a crest, an uneven landing, and constant-radius turns. Log wheel contact, compression, normal load, slip, and chassis angular velocity. Confirm no force spikes or unsupported traction across a representative speed range.

### Author distinct ground and air assists

Define when enough wheel or support contacts exist for ground steering. Set separate limits for air rotation, downforce, and recovery behavior. Ramp assists around takeoff and landing when a sudden mode switch produces sharp impulses. For a flight or water vehicle, express analogous controls in terms of thrust and orientation, and drive support through lift or buoyancy appropriate to the game.

**Check** on banked surfaces, ramps, surface transitions, and interrupted landings. Compare input-to-rotation response with assists enabled and disabled and trace which effect supplied the force or torque.

### Reproduce handling and recovery

Record tick-indexed steering, throttle, and brake values with initial state, surface, vehicle setup, and applicable assists. Give the test track a straight, a constant-radius turn, a surface transition, and a jump. For reset behavior, choose a valid respawn pose and define the velocity the vehicle resumes with.

**Check** by replaying a stable baseline after tuning changes and at different rendering rates. Compare lap segments and high-percentile simulation cost. Trigger reset while overturned, off track, intersecting a prop, and in multiplayer if supported.
