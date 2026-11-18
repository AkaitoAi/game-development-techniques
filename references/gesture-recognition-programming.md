# Gesture recognition programming

## Problem and design reasoning

Tap, drag, swipe, and long-press interpretations often share the same initial touch samples. Committing too early causes accidental actions, while waiting for every possible gesture makes a control feel slow. Zach Gage's GDC 2012 talk discusses *Fruit Ninja* as a fast game using analog swipe controls and explains how tactile response shapes touch interaction. Blizzard documented a purpose-built phone interface for shipped *Hearthstone*, while Derek Sakamoto's GDC 2015 talk discusses the game's interface process. These sources motivate evaluating the interaction in its actual gameplay and screen context. [Gage, Controls You Can Feel, GDC 2012](https://media.gdcvault.com/gdc2012/slides/Summit_Smartphone%20%26%20Tablet/Gage_Zach_ControlsYouCanFeel.pdf) · [Blizzard mobile release](https://worldofwarcraft.blizzard.com/en-gb/news/18782302/hearthstone-now-available-on-mobile) · [Sakamoto, Hearthstone UI, GDC 2015](https://www.gdcvault.com/play/1022036/)

The architecture below is a portable recommendation derived from that interaction problem. The cited talks support the design context and do not establish these games' internal recognizer implementations.

## Techniques and practical checks

- Store each pointer's stable ID, initial position, timestamp, recent samples, and capture owner. Give recognizers possible, accepted, cancelled, and completed outcomes. Define when one recognizer excludes another. **Check** a tap that becomes a drag, a second finger arriving late, and a finger leaving the display. Verify each pointer has one terminal outcome and no unrelated pointer inherits its state.
- Define thresholds in device-appropriate units and use event timestamps when measuring duration or velocity. Keep the recognizer's numerical thresholds separate from gameplay scoring. **Check** replay equivalent physical motions on small and large screens at different refresh rates. Compare classification, activation delay, and unintended actions.
- Separate reversible preview from committed gameplay. A card can follow a finger while the recognizer is tentative, with a legal drop committing the move. **Check** drag into and out of a legal target, release over an invalid target, and cancel during preview. Verify the preview returns cleanly and costs or rewards apply once.
- Treat focus loss, orientation change, UI replacement, and interrupted touch streams as explicit cancellation paths. Emit cancellation to the owner and release capture. **Check** receive an app interruption midway through a long press or multi-touch gesture, then resume. Verify no delayed activation occurs and a new gesture starts from fresh state.

See [Touch controls](touch-controls.md) for layout and [Mobile input latency](mobile-input-latency.md) for end-to-end measurement.
