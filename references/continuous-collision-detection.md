# Continuous collision detection

## Problem and design reasoning

A body can cross a thin obstacle between two discrete simulation states. Extra collision fidelity costs time, and enabling it indiscriminately may put the physics step over budget. Catto's GDC 2013 talk documents *Diablo III* using time-of-impact handling and substeps for dynamic-versus-static contacts to keep thrown ragdolls and debris inside the world. It also explains trade-offs of speculative contacts and stopping at impact. [Catto, Continuous Collision, GDC 2013](https://box2d.org/files/ErinCatto_ContinuousCollision_GDC2013.pdf)

*Rocket League* provides a different shipped choice. Jared Cone documents discrete collision detection at a fixed 120 Hz and explains how smaller steps improved hit consistency at a CPU cost. The portable design inference is to choose collision coverage from supported relative motion, geometry, and gameplay consequences, then measure the selected strategy. [Cone, It IS Rocket Science!, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Cone_Jared_It_Is_Rocket.pdf)

## Programming techniques and practical checks

### Define collision coverage per interaction

List fast bodies and their critical collision partners. Verify whether the selected engine mode handles static targets, dynamic targets, angular motion, and sensors. Use travel per step relative to obstacle thickness as a warning signal, including the sweep of a rotating shape.

**Check** a thin wall, a crossing moving target, and a long rotating body at the highest supported speeds. Confirm the expected pair types are covered and log modes for missed contacts.

### Choose the permitted motion response

For a detected impact, decide whether to stop, simulate the remaining fraction, or use the solver's speculative constraints. Bound iterations and define overload behavior when several impacts occur in one step.

**Check** a bounce corridor and several nearby contacts in one tick. Compare elapsed simulated time, penetration, and visible hitching while measuring maximum solver work.

### Separate detection from gameplay event delivery

Determine when the engine reports continuous contacts and which callback identifies the accepted outcome. Use stable body and action identities to prevent repeated contact processing from awarding several hits.

**Check** one fast hit across several substeps, destruction during impact handling, and repeated playback of the same input trace. Verify one gameplay result per intended contact.

### Validate fidelity and budgets together

Capture a discrete baseline and selected continuous modes on the same route. Use the lowest-cost mode that passes the project's collision requirements, and include contact density in the performance fixture.

**Check** normal play and a burst of many fast bodies on target hardware. Report missed contacts, worst penetration, step-time tails, and overload frequency before accepting the configuration.
