# Game input command history

## Problem and design reasoning

Input arrives at a different cadence from simulation. When a server correction arrives, the client needs the commands issued after that correction's baseline. Re-reading current device state loses earlier presses and timing, while keeping unbounded history makes memory and replay cost unpredictable.

In shipped *Quake III Arena*, client prediction starts from a snapshot, reads a bounded sequence of commands, and skips commands already represented by the snapshot's command time. If the necessary history is gone, prediction returns without reconstructing from an incomplete sequence. [id Software, Quake III Arena client prediction source](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_predict.c#L432-L485)

Psyonix documents *Rocket League* recording inputs with frame numbers, receiving server physics state associated with a client frame, and replaying subsequent frames on correction. Jared Cone explains that higher latency increases correction work and that server input buffering helps absorb timing variation. The portable inference is to size histories for an explicit latency and CPU budget and design for what happens outside it. [Jared Cone, It Is Rocket Science: The Physics of Rocket League, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Cone_Jared_It_Is_Rocket.pdf)

## Techniques and practical checks

- **Store normalized simulation commands.** Convert device samples into immutable tick-indexed commands with a sequence identifier, bounded axes, and the needed button state or edges. Specify how several device samples contribute to a tick. **Check** a quick press and release between two ticks and a held input through multiple render rates. The captured command trace should reflect the game's intended handling in each case.
- **Retire only what the baseline represents.** Tie an authoritative snapshot to the last command it incorporated and replay later commands in sequence. Keep transport acknowledgments distinct if they describe receipt rather than simulation. **Check** a delayed snapshot after newer input has been sent and a duplicate older snapshot after correction. Verify that commands are neither skipped nor applied twice.
- **Bound both history and catch-up.** Use a fixed-size history based on the supported correction window and measure the worst-case replay cost. If a correction predates the contiguous history, adopt a defined resync policy. **Check** latency and loss inside the supported envelope, then exceed it deliberately in a test session. Log buffer occupancy and resimulation steps and verify recovery from an unavailable baseline.
- **Scope commands to a control lifetime.** Tag the history with the current match and possession or spawn generation. On a discontinuity, invalidate incompatible commands and define whether held input carries over. **Check** respawn, vehicle entry, and reconnect with a button held and a late response in flight. Earlier commands should have no effect on the new control target.

See [Multiplayer movement](multiplayer-movement.md) for the broader prediction model and [Simulation record-and-replay testing](simulation-record-and-replay-testing.md) for longer regression traces.
