# Streaming audio programming

## Problem and design reasoning

Long audio cannot always remain decoded in memory, while slow reads or decode jobs can starve playback. Rockstar's *Grand Theft Auto V* presentation calls out predictable streaming and describes timing guarantees including source chaining and decode-latency compensation. Chase Thompson's GDC 2016 session on *Halo 5: Guardians* identifies streaming bandwidth, memory, and middleware integration among the production constraints. These sources establish the shipped problems. The bounded-buffer protocol below is an engine-independent recommendation. [Rockstar North, GDC 2014](https://media.gdcvault.com/GDC2014/Presentations/MacGregor_Alastair_The_Sound_of.pdf) · [343 Industries, Managing Audio Performance, GDC 2016](https://gdcvault.com/play/1023229/Managing-Audio-Performance-Lessons-Learned)

## Techniques and practical checks

### Separate reading, decoding, and consumption

Use a bounded set of buffers with explicit states for read pending, decode pending, ready, submitted, and reusable. Assign a generation to the stream and retain storage until the audio backend finishes consuming it. Schedule disk and decode work outside the audio callback. Microsoft's streaming example uses separate work and completion signaling. [Microsoft streaming audio guidance](https://learn.microsoft.com/en-us/windows/win32/xaudio2/xaudio2-streaming-audio-data)

**Check** delayed reads, out-of-order completion, and delayed buffer completion. Trace each buffer's owner and generation and verify that playback never consumes data still being written or storage already recycled.

### Budget startup and steady-state headroom

Measure the read and decode tail latency for each platform. Choose prebuffering and low-water thresholds that cover the intended workload within memory limits. Give imminent playback deadlines priority and define an underrun response appropriate to the sound, such as a controlled fade or a restart at a valid boundary.

**Check** a cold launch, competing asset loads, and the largest supported simultaneous streams. Report startup delay, underruns, peak decoded memory, and minimum queued audio duration.

### Treat seeking and cancellation as new generations

Associate each seek or restart with a new request generation. Discard stale completions and reconstruct decoder state at a supported boundary before publishing replacement audio. Give loops explicit sample positions and handle codec delay or padding where the format requires it.

**Check** rapid seeks followed by cancellation and immediate reuse of the same playback object. Verify the final position, absence of old audio, and continuous loop boundaries in a captured waveform.

### Retire streams after consumption completes

Stop issuing new work, cancel or drain outstanding requests, and keep submitted buffers alive until completion. Distinguish playback stopped from resources fully retired. Keep the completion path free of blocking disk work. [Microsoft XAudio2 callback guidance](https://learn.microsoft.com/en-us/windows/desktop/xaudio2/xaudio2-callbacks)

**Check** scene unload, device interruption, and repeated start-stop cycles while reads are pending. Confirm callbacks cannot reference freed state and that buffer and decoder counts return to the baseline.
