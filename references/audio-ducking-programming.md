# Audio ducking programming

## Problem and design reasoning

A priority decision must become a controlled gain change on the audio thread. If each event writes a mixer gain independently, an old release can restore the mix while a new important event is active. Jonathan Lanier's GDC talk describes preserving important dialogue and effects within the immersive mix of *The Last of Us*. Naughty Dog's audio team also describes mixing throughout production because dynamic controls interact. The token and envelope protocol below is a portable recommendation. [Jonathan Lanier, Aural Immersion, GDC 2014](https://www.gdcvault.com/play/1020444/Aural-Immersion-Audio-Technology-in) · [Naughty Dog, The Mix in The Last of Us](https://www.audiogang.org/wp-content/uploads/2014/09/GANG-IESD-BEST-MIX-2013-INTERVIEW-NAUGHTY-DOG-THE-LAST-OF-US.pdf)

## Techniques and practical checks

### Give duck requests owners

Represent a request with an owner, affected buses, target attenuation, attack, hold and release. Define whether overlapping requests choose maximum attenuation or combine within a limit.

**Check** Start two requests, finish the first, and verify the second still controls attenuation. Finish both and verify return to the base mix.

### Advance envelopes on the right clock

Evaluate ramps continuously from the current gain. Define audio behavior during gameplay pause, slow motion and device interruption.

**Check** Interrupt attack and release with a new request. Record the output and check for gain steps, pumping and clicks.

### Separate base mix and duck contribution

Combine user volume, authored state and temporary attenuation in one documented order. Hand compact request changes to audio execution safely.

**Check** Move the music slider during a duck, then release it. Verify the selected user level remains the baseline.

### Retire requests reliably

Release or expire requests when their emitter, dialogue, scene or playback instance ends. Use identity that survives delayed callbacks without confusing a replacement.

**Check** Destroy an emitter during hold, reuse its object, and deliver the old completion. Confirm the replacement is unaffected and all temporary gain changes clear.
