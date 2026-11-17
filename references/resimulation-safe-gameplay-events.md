# Resimulation-safe gameplay events

## Problem and design reasoning

Re-executing a simulation tick can repeat an effect that already played, or invalidate an effect the player already heard. A replayed tick can also repeat an irreversible operation if the simulation writes directly to persistence. In *Mortal Kombat XL* and *Injustice 2*, NetherRealm rolled back and resimulated gameplay while reusing equivalent sounds and particles. Its GDC slides explain that this avoids recreating those objects and reduces how much presentation state needs serialization. GGPO creator Tony Cannon describes comparing the audio queue with the corrected simulation to account for effects that should now stop. A transferable design inference is to reconcile the effects a tick requests and give persistent outcomes a separate commit boundary. [Michael Stallone, 8 Frames in 16ms, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Stallone_Michael_8FramesIn16ms.pdf) · [Tony Cannon, Fight the Lag, Game Developer](https://media.gdcvault.com/GD_Mag_Archives/GDM_September_2012.pdf)

## Techniques and practical checks

### Give emitted events a reproducible identity

Record the tick, stable source identity, occurrence within that source, and semantic event type. Compare payloads too when a correction can change the target or position. Choose an occurrence key that distinguishes legitimate same-tick repeats and stays stable when unrelated events are inserted.

**Check** replay a tick with two identical-looking hits and one unrelated event that disappears after correction. Confirm the two hits remain distinct and previous equivalent effects are reused.

### Reconcile effects after resimulation

Compare old and rebuilt event histories over the corrected range. Keep equivalent effects, start newly valid effects as appropriate, and stop or fade invalidated long-lived effects. Choose a late-start policy for one-shots and allow immediate predictions for effects whose occasional error is tolerable.

**Check** predict an impact that is later denied, then repeat with a correction that changes only its target. Verify that presentation converges and valid effects outside the corrected range stay active.

### Defer irreversible outcomes to confirmation

Keep speculative entries inside reversible simulation state. Publish persistent rewards, achievement progress, and durable audit records only after the relevant authoritative event reaches its commit boundary. Use a durable event identifier with an idempotent consumer when delivery can retry.

**Check** simulate repeated corrections around an unlock, then simulate delivery retries and a restart in a test environment. Confirm the invalid branch grants nothing and the committed event grants once.

### Verify state and side effects together

Record inputs, snapshots, and event decisions for a reproducible run. Keep the presentation registry bounded by the relevant correction horizon and the lifetime of active effects.

**Check** compare a baseline run with runs that force repeated corrections over the same ticks. Compare the confirmed gameplay outcomes and inspect audio, particle, and persistent event counts. Repeat at the oldest supported correction boundary and after session teardown.
