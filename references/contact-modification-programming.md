# Contact modification programming

## Problem and design reasoning

Some interactions need a different solver response even though the shapes should still touch, such as a conveyor surface or a one-way platform. A collision mask alone cannot express that response. In the engine shipped with *Horizon Forbidden West*, Jolt's contact listener exposes settings before constraint resolution. Jorrit Rouwe's GDC 2022 talk explains the engine's streaming and multithreaded-update goals. Current listener documentation also specifies callbacks running concurrently while bodies are locked. Those API details describe Jolt's contract, rather than proving which specific contact overrides Guerrilla used. [Guerrilla, Architecting Jolt Physics, GDC 2022](https://gdcvault.com/play/1027891/Architecting-AsterCore-Physics-for-Horizon) · [Jolt contact listener contract](https://jrouwe.github.io/JoltPhysics/class_contact_listener.html)

PhysX documents modifying contacts between detection and solving, including surface velocities and selective impulse limits. The portable programming inference is to keep pre-solver policy small, deterministic, and separate from post-solver gameplay side effects. [NVIDIA, advanced collision detection](https://nvidia-omniverse.github.io/PhysX/physx/5.1.1/docs/AdvancedCollisionDetection.html)

## Techniques and practical checks

### Define a response policy per interaction

Record which pair categories can override friction, restitution, surface motion, or response participation. Decide the evaluation order when both objects contribute a policy. Preserve a meaningful physical contact unless the authored rule explicitly excludes it. Keep the policy inputs in a snapshot that remains valid throughout the simulation step.

**Check** a conveyor carrying a resting object, an opposing moving object, and two touching conveyors with different directions. Log the selected policy and verify the result at the supported tick rates.

### Treat one-way contact as a stateful rule

Classify approach direction using a documented reference frame and clearance margin. Include the object's previous side where the design needs continuity through a thin surface. Give intentional drop-through a bounded state with a clear exit condition.

**Check** approach from above and below, start overlapping, rotate the supporting surface, and drop through near an edge. Verify the character can regain support and cannot remain stuck in an ignored-contact state.

### Respect the solver callback contract

Apply only the modifications permitted by the engine at that phase. Queue copied gameplay observations for delivery after the step. Carry stable object identities and revisions instead of retaining a pointer to contact memory. Use the engine's documented threading and locking rules.

**Check** many simultaneous contacts while objects are destroyed or returned to a pool after the step. Verify that deferred observations resolve to the original object generation and that no callback blocks on a lock already owned by the solver.

### Validate repeated and continuous contacts

Define what happens on new, persisted, removed, and continuous-collision callbacks. Count gameplay outcomes independently from the number of low-level contact notifications. Re-evaluate policy when a relevant material or ability state changes.

**Check** a sliding contact over several substeps, a fast collision, a sleeping body waking, and a material change during contact. Compare physical response and gameplay event counts, then profile the worst contact-dense scene.
