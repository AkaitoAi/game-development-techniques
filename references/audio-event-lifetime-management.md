# Audio event lifetime management

## Problem and design reasoning

An audio event may contain a delayed start, several sounds, or a tail that outlives its gameplay source. Destroying an actor does not automatically define what should happen to all of those parts. The *Halo 5: Guardians* team describes engine and middleware integration as part of its CPU, memory, streaming, and content management work. A reliable ownership contract makes those costs and cleanup paths inspectable. [343 Industries, Managing Audio Performance, GDC 2016](https://gdcvault.com/play/1023229/Managing-Audio-Performance-Lessons-Learned)

Audiokinetic documents game-object registration, per-object state, and explicit unregistration when the object is no longer needed. Playback also has identities that can be used for targeted operations. A transferable inference is to track an event request separately from the actor that requested it and from the physical voices it currently uses. These lifecycle recommendations extend the documented APIs and are not a claim about *Halo 5*'s internal event protocol. [Wwise Fundamentals, game objects](https://www.audiokinetic.com/files/?get=2015.1.9_5624%2FWwise_Fundamentals_en.pdf) · [Wwise release notes, playing-ID operations](https://www.audiokinetic.com/files/?get=2014.1.6_5318%2FWwise_v2014.1.6_ReleaseNotes.pdf)

## Techniques and practical checks

- Define an event record with request identity, owner generation, playback identity, start state, and stop policy. Decide whether the sound follows its actor, transfers to a world-space tail, or fades on destruction. **Check** destroy the source before a delayed start and during a long tail. Confirm each event follows the intended policy and its record is eventually retired.
- Treat completion as an asynchronous message. Queue only stable callback data, validate generation at the gameplay-thread handoff, and make stop and completion safe to observe more than once. **Check** stop an event just as it completes, then immediately reuse the actor. Confirm the old completion cannot change the new actor's playback or invoke its gameplay callback.
- Keep an ownership list for cancellable loops and scheduled events. On pooling or scene exit, cancel delayed work, retire or transfer active events, and reset per-owner parameters before reuse. Preserve explicit tails when their design requires them. **Check** repeatedly enter and leave a scene containing delayed ambience and loops. Verify event records, registrations, and pending callbacks return to a stable baseline.
- Instrument event creation, start, stop request, completion, and owner retirement with the same trace ID. State what happens if content fails to load or a scene shuts down before playback begins. **Check** a missing asset, rejected event, shutdown during load, and multiple stop requests. Require one terminal outcome for each accepted request and enough trace context to explain it.

See [Audio voice allocation](audio-voice-allocation.md) for playback-slot budgets and [Runtime audio parameter control](runtime-audio-parameter-control.md) for parameter ownership.
