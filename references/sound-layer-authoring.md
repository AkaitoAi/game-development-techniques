# Sound layer authoring

## Problem and design reasoning

A convincing sound can contain several recordings whose jobs differ. The attack communicates contact, the body communicates weight, mechanisms identify the device, and the tail suggests the space. In its *Halo Infinite* audio breakdown, 343 Industries describes the Skewer using impact, mechanical, science-fiction, and environmental tail components. The team also explains that excessive detail in earlier games could obscure important threats. Its reasoning connects each ingredient to identity and gameplay readability. [343 Industries, Inside Infinite, March 2021](https://www.halowaypoint.com/en-us/news/inside-infinite-march-2021)

Chase Thompson's GDC 2016 talk *Managing Audio Performance: Lessons Learned from the Production of Halo 5: Guardians* covers the production constraints of CPU, memory, streaming, and content management. That context matters when deciding which components remain independently controllable at runtime. The layer contracts and tests below are portable recommendations rather than a reconstruction of either game's implementation. [GDC session](https://gdcvault.com/play/1023229/Managing-Audio-Performance-Lessons-Learned)

## Techniques and practical checks

### Assign a semantic job to each layer

Write the information each component contributes, its timing anchor, perspective, and optionality. Keep a recognizable core that survives when decorative components are removed. Decide which layers are baked together and which need independent playback because their environment or gameplay state changes.

**Check** mute each component in a representative encounter. Record the information lost. Remove or simplify components that add cost without a useful audible distinction.

### Preserve timing and phase deliberately

Align layers against the action's contact or firing marker. Inspect attacks and low-frequency components for unintended cancellation. Choose correlated variants when their timing must fit together, and independent selection when combinations remain compatible.

**Check** audition the permitted combinations in mono and stereo, including the lowest intended playback level. Confirm attack timing and perceived weight remain inside the authored range.

### Author perspective and environment substitutions

Define how close, remote, interior, and exterior playback select components. Preserve action identity through the transition. Use a defined crossfade or tail policy when the source changes spaces so one event does not acquire contradictory reflections.

**Check** fire or trigger the event while crossing an interior boundary and moving through distance bands. Confirm no duplicate attack, abrupt missing tail, or misleading distance cue.

### Budget the assembled event

Measure the full event's peak, voice count, and duration. Mark essential components for prioritization, and define graceful omission for optional components under load. Include the result in a crowded gameplay mix.

**Check** play the maximum expected concurrent events with dialogue and threat cues. Verify essential information survives voice pressure, the output remains within the project's loudness policy, and optional layer removal is audible only within accepted limits.

See [Sound variation authoring](sound-variation-authoring.md) for variant selection.
