# Environmental reverb authoring

## Problem and design reasoning

A space may look like stone, fabric, or open terrain yet sound identical to every other location. Conversely, excessive reverberation can hide speech and enemy cues. This guide concerns the authoring and listening workflow for room character, sends, and tails. Use spatial acoustics for propagation paths and runtime audio zones for membership logic.

**Hitman 2** provides a documented shipped example. IO Interactive and Intel describe rooms as areas with similar acoustic properties, including outdoor areas, and discuss limitations of applying only the listener room's response. Their improved system accounts for where sound originates. The reasoning is perceptual: reverberation conveys environmental information, while exact physical simulation is too expensive. [IO Interactive and Intel, Hitman 2: Enhancing Reverb on Modern CPUs](https://www.intel.com/content/www/us/en/developer/articles/case-study/hitman-2-enhancing-reverb-on-modern-cpus.html)

Stepan Boev's **Sound Propagation in Hitman**, presented at GDCE 2015, describes a workflow supporting open and enclosed spaces and dynamic geometry. Its public description establishes the problem of misleading distance-only attenuation; it does not document every artistic preset decision. The authoring checks below are recommendations informed by that separation of spatial structure and audible result. [IO Interactive, GDCE session](https://gdcvault.com/play/1022824/Sound-Propagation-in)

## Techniques and practical checks

### Author a small family of acoustic intentions

Define recognizable room families before creating a unique preset for every volume: dry furnished room, reflective corridor, large hall, sheltered exterior, and open exterior are possible starting points. Record intended early-reflection character, late decay, damping, and wet/direct balance. Attach listening references and explain which gameplay cues must remain legible. Numeric presets are starting points, not proof of acoustic correctness.

**Check** audition the same short impulse, footsteps, speech, and sustained effect through every family at matched direct level. A reviewer should recognize the intended distinction without seeing the level, and speech must remain understandable.

### Review emitter and listener combinations

Build a listening matrix for source inside/listener inside, source outside/listener inside, the reverse, and adjacent rooms. Decide which space contributes the audible tail and how a doorway influences apparent arrival. An implementation may approximate these cases, but the author must hear the approximation rather than tuning only a listener standing beside the source.

**Check** keep an emitter fixed while moving through a doorway, then reverse the roles. Listen for the source inheriting an implausible listener-only room character or a large hall becoming a dry booth when observed from outside.

### Budget sends by sound role

Separate dialogue, locomotion, weapons, ambience, and exceptional dramatic effects where the mixer supports it. Give each role an intentional send range and filtering policy. Avoid both universal dryness and applying the maximum room response to every event. Compare the full mix with isolated effects before increasing decay to make a room feel bigger.

**Check** play a representative combat encounter with simultaneous dialogue and a required navigation cue. Compare intelligibility and cue localization with reverb bypassed and enabled; adjust sends or spectral balance when added character masks essential information.

### Validate boundaries and tail budgets together

Author blending distance, priority, tail retention, and fallback behavior alongside room presets. Place test points around doors, vertical overlaps, and streaming boundaries. Keep a maximum concurrent processing budget; decide how low-priority tails degrade before measuring worst-case traversal.

**Check** sprint repeatedly between contrasting rooms while several sounds decay, then teleport and unload a region. Listen for pumping and abruptly truncated tails, record active reverb instances and processing time, and confirm the fallback still preserves essential room distinctions.
