# Runtime debug command systems

## Problem and design reasoning

Developers need repeatable ways to put a game into a useful test state. A text command that mutates half a scenario before rejecting an argument can make diagnosis less reliable. Quake's released command module registers named commands, tokenizes arguments, and processes a command buffer. In his Mortal Kombat 1 GDC talk, NetherRealm's Etienne Palmer-Campbell explains how specialized testing tools helped developers test complex combat independently. The portable command contracts below are recommendations inferred from the need for repeatable setup, rather than claims that either game implemented them exactly. [id Software, Quake command processing](https://github.com/id-Software/Quake/blob/master/WinQuake/cmd.c) · [Palmer-Campbell, Mission Kontrol: Revolutionizing Combat Testing for Mortal Kombat 1, GDC 2025](https://media.gdcvault.com/gdc2025/Slides/Palmer-Campbell_Etienne_MissionKontrolRevolutionizing.pdf)

## Techniques and practical checks

### Register commands with typed contracts

Describe each command's name, argument types, allowed range, availability, and owner. Parse and validate arguments before changing gameplay. Return useful errors with the rejected value and accepted form. Expose the same command handler through a console or test panel where useful.

**Check** omitted arguments, unknown names, out-of-range values and malformed entity IDs. Verify that rejected commands produce a useful message and leave the world in its prior state.

### Schedule mutation at a safe phase

Execute gameplay mutations at a defined simulation boundary. Resolve entity handles at execution time and recheck conditions that may have changed since submission. For multi-step setup, validate prerequisites and define a commit point or compensating cleanup.

**Check** queue a command against an actor that despawns before execution, submit two conflicting commands in one frame, and interrupt a scenario setup. Verify a clear terminal result and consistent state.

### Trace the state a command creates

Record command name, normalized arguments, execution tick, content revision and outcome. Provide a way to export a short sequence of setup commands for a bug report. Capture the actual seed or selected entity rather than leaving an implicit random choice in the report.

**Check** reproduce a scenario from its exported setup in a fresh process. Compare relevant world state and verify that failed commands can be distinguished from commands that were never executed.

### Give registration an explicit lifetime

Register session-specific commands when their owning subsystem becomes available and remove them when it shuts down. Gate developer mutations by build or authorized development mode, with the availability visible in help. Keep read-only inspection commands distinct in their declared capabilities.

**Check** reload a scene, reconnect a session, and restart the owning subsystem. Confirm each command appears once, resolves its current owner, and cannot call a retired object. Verify the release build exposes only its intended command set.

See [Runtime debugging tools](runtime-debugging-tools.md) for choosing useful observations and [Game command undo and redo](game-command-undo-and-redo.md) for reversible authoring commands.
