# Gameplay action cancellation

## Problem and design reasoning

An action may reserve a target, start an animation, consume a resource, and produce an outcome at different moments. Cancelling its scheduled work does not define which of those gameplay changes should survive. In Valve's released Source SDK combat weapon code, AbortReload explicitly clears reload state and stops the reload sound on the client. That is a concrete cleanup contract for a shipped weapon action. [Valve, Source SDK combat weapon implementation](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/shared/basecombatweapon_shared.cpp)

Dan Reed's GDC presentation describes Statescript driving weapons and abilities in shipped *Overwatch*, with prediction and replication automated around the scripted behavior. An action's authoritative result and its local presentation can therefore have separate lifetimes. The phase, outcome, and cleanup protocols below are portable recommendations motivated by these problems. The sources do not establish that either game used this exact protocol. [Dan Reed, Networking Scripted Weapons and Abilities in Overwatch, GDC 2017](https://gdcvault.com/play/1024653/Networking-Scripted-Weapons-and-Abilities)

## Techniques and practical checks

### Define cancellability by phase

Give an action an instance identity and explicit phases such as preparing, committed, recovering, and terminal. Specify which interruptions each phase accepts. Define whether cancellation before commit refunds a reservation, while cancellation after commit ends presentation or recovery without reversing an already resolved outcome. Route a cancellation request through the state owner.

**Check** request cancellation immediately before, on, and after the commit tick. Record the final outcome, resource delta, target reservation, and cooldown. Verify each case matches the phase policy and reaches one terminal state.

### Track temporary control as owned grants

Record the action's movement restriction, camera override, interaction reservation, and temporary effect as grants or tokens that can be released individually. Cleanup releases only what the retiring action still owns. Transfer ownership explicitly when a follow-up action inherits a resource.

**Check** replace action A with B while both need movement restriction, then deliver A's delayed cancellation. Verify B retains its restriction, A's other grants are released, and normal input returns after B ends.

### Settle cancel-versus-complete races

Process completion and interruption against the action's current identity and phase. Assign one result when both arrive in the same simulation step, with a documented precedence. Keep cleanup repeatable and validate delayed animation, timer, or network callbacks before they mutate state.

**Check** enqueue complete and cancel in both arrival orders for the same action, repeat either event, and reuse the actor. Verify one result and no callback affecting the replacement action.

### Recover through the normal action entry point

Report a meaningful cancellation reason to the requesting system and clear pending input according to the game's buffering rule. Verify the next legal action can start through ordinary controls. Distinguish a gameplay cancellation from an owner disappearing during scene teardown.

**Check** interrupt an interaction with movement, target destruction, pause/resume, and scene exit. Resume play where applicable and start the next interaction. Confirm bounded recovery and that the previous action leaves no active reservation or looped presentation.

See [Gameplay coroutines and tasks](gameplay-coroutines-and-tasks.md) for continuation lifetimes and [Gameplay transaction boundaries](gameplay-transaction-boundaries.md) for committed state changes.
