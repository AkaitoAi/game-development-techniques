# Physics constraints and joints

## Problem and design reasoning

A hinge, piston, cable, or articulated assembly needs a defined relationship between bodies while still reacting to forces and collisions. Directly setting both bodies' transforms can fight the solver and produce jitter or energy spikes. In shipped *Space Engineers*, rotors and pistons connect independently simulated constructions. Jan Hlousek's GDC 2023 talk discusses the added networking problems when characters and vehicles move inside larger moving structures. Nintendo's GDC 2024 presentation describes building *Tears of the Kingdom* around physics-driven interactions. These cases motivate treating an assembly as a persistent gameplay object with explicit attachments and state. The recipes below are implementation guidance inferred from those problems, rather than claims about either game's exact joint implementation. [Keen Software House, Predicted Physics-Based Multiplayer in Space Engineers, GDC 2023](https://www.gdcvault.com/play/1029260/Online-Game-Technology-Summit-Predicted) · [Nintendo, Tunes of the Kingdom, GDC 2024](https://www.gdcvault.com/play/1034667/Tunes-of-the-Kingdom-Evolving)

## Techniques and practical checks

### Define the constraint in body-local frames

Store each anchor and axis in the relevant body's local space. Declare which translation and rotation axes are free, limited, driven, or locked. Keep the authored definition separate from the runtime joint handle, and resolve body identities before creating the joint. Convert coordinates once at the boundary where the physics engine expects them.

**Check** a joint with rotated and translated parent objects, then save and restore the assembly. Visualize both anchor frames and measure separation and axis alignment before and after the first simulation step.

### Tune the supported load envelope

Author plausible mass and inertia, then tune solver settings and substeps against the actual supported geometry and loads. For a motor, specify speed or position target, force or torque limit, and whether reaching a stop can break the joint. Use a documented conversion for any engine-specific spring frequency, stiffness, or damping parameters. [Box2D joint and solver guidance](https://box2d.org/documentation/md_simulation.html)

**Check** the smallest and largest supported assemblies, including a long chain, closed loop, extreme allowed mass ratio, and a motor pushing into a stop. Record anchor error, penetration, energy growth, and high-percentile simulation cost.

### Make attachment changes explicit

Queue creation, destruction, and parameter changes for a supported physics mutation phase. Give the gameplay owner responsibility for deleting the runtime joint before its body handles become invalid. Decide whether connected bodies collide and restore any temporary collision exception when detaching.

**Check** attach and detach while the assembly moves, destroy either body, pool and reuse a body, and issue duplicate detach requests. Confirm one break notification, valid collision policy after detachment, and no stale handles.

### Replicate assembly intent and recovery

Represent attachments by stable body IDs and local frames. In a networked game, choose one authority for attachment changes and identify the simulation tick at which they apply. Define how a late-joining or corrected client reconstructs an assembly and avoids showing a half-created connection.

**Check** join during a moving assembly, delay an attach message, and correct a body after an impact in an authorized local test. Compare attachment topology and motor state, and measure visible recovery time and resimulation cost.
