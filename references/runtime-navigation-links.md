# Runtime navigation links

## Problem and design reasoning

A path through a ladder, bridge, jump, or moving platform represents an action with conditions, rather than ordinary walking. In shipped *Death Stranding*, player-placed bridges and ladders changed the connectivity that AI had to navigate. Eric Johnson's developer talk describes supporting those dynamic structures alongside difficult terrain. *Horizon Zero Dawn* likewise required navigation and animation changes for creatures with different dimensions. These cases support making traversal capability and execution explicit. The lifecycle and reservation protocol below is an implementation recommendation inferred from those requirements. [Eric Johnson, Death Stranding AI Postmortem, GDC 2021](https://gdcvault.com/play/1027144/AI-Summit-Death-Stranding-An) · [Julian Berteling, Beyond Killzone, GDC 2018](https://gdcvault.com/play/1024912/Beyond-Killzone-Creating-New-AI)

## Techniques and practical checks

### Register a link with a complete traversal contract

Store stable identity, entrance and exit frames, direction, supported actor dimensions, required abilities, capacity, and estimated traversal cost. Validate clearance and usable approach surfaces when publishing the link. Keep the link tied to the source object's lifetime.

**Check** a small actor, a large actor, an actor lacking the ability, and approach from the reverse direction. Verify that search eligibility and execution use the same criteria.

### Separate route selection from execution

Pathfinding selects the link. A traversal action owns approach, alignment, commitment, movement, and completion. Define which system owns displacement during each phase and which events permit interruption. Return to ordinary path following only after placing the actor on a valid exit surface.

**Check** interrupt before entry, during traversal, and immediately before exit. Inspect root motion, collision, and navigation ownership. Verify the actor resumes with one movement owner and a legal position.

### Reserve capacity without indefinite waiting

For narrow links, use reservations with expiry and release rules. Let waiting actors select a usable staging location and replan after a bounded wait. Update occupancy when an actor dies, disconnects, or unloads.

**Check** send two groups in opposite directions, remove a reserved actor, and stall an actor midway. Record wait duration and throughput. Verify reservations release and actors find the intended fallback.

### Invalidate topology and handle active users

When a structure moves or disappears, invalidate future traversal and affected routes. Define separately how an already committed user finishes, aborts, or recovers. Re-register replacement structures with a new generation so old callbacks cannot operate on a reused identity.

**Check** remove or move the source structure while an actor is approaching and while it is committed. Save and restore around the change. Confirm every actor follows the documented recovery rule and that the removed link stays unavailable.
