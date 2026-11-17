# Watercraft and buoyancy

## Problem and design reasoning

Players read the hull, horizon, and deck together. A convincing wave surface can still produce a poor sailing experience if the hull vibrates, deck contacts slip, or a tall wave looks small from the helm. Naughty Dog reported choosing waves over animation loops to drive boats and barges in *Uncharted 3*, and increasing wave amplitude so storms read from an elevated camera. Grapeshot Games and NVIDIA reported that applying forces directly from sampled waves introduced noise and instability in *ATLAS*, where servers often ran under 10 Hz. They fit a plane to the samples and used a spring to approach that pose. An inference for other projects is to choose the amount of simulation by asking which water interactions affect player decisions and which only need to look coherent. [Carlos González-Ochoa, Water Technology of Uncharted, GDC 2012](https://media.gdcvault.com/gdc2012/slides/Programming%20Track/Gonzalez-Ochoa_Carlos_Water_Technology_Uncharted.pdf) · [Mark Mihelich and Tim Tcheblokov, Interactive Water Simulation in ATLAS, GDC 2019](https://gpuopen.com/gdc-presentations/2019/gdc-2019-agtd6-interactive-water-simulation-in-atlas.pdf)

## Shipped examples

- **Uncharted 3: Drake's Deception (2011).** Naughty Dog's presentation describes a stateless parametric wave system. The ocean could be evaluated for a given position and time, including when cinematic tools needed to rewind. The boats responded to those waves. [Naughty Dog, GDC 2012](https://media.gdcvault.com/gdc2012/slides/Programming%20Track/Gonzalez-Ochoa_Carlos_Water_Technology_Uncharted.pdf)
- **ATLAS (public early access, 2018).** The developers describe evaluating sea displacement at several positions along the hull, fitting a plane, and using it to drive a filtered target transform. The talk also explains why recovering water height from a horizontally displaced surface can require iteration. [Grapeshot Games and NVIDIA, GDC 2019](https://gpuopen.com/gdc-presentations/2019/gdc-2019-agtd6-interactive-water-simulation-in-atlas.pdf)

## Techniques and practical checks

### Define the water query

Give gameplay a bounded-cost way to query height, surface velocity, and any relevant normals at a world position and simulation time. If horizontal displacement changes the surface position, account for it in the query or validate that the approximation is acceptable. Keep cosmetic detail on a separate scale from waves that can move the hull.

**Check** draw markers for physics samples on calm water, steep crests, and near shore. Record the difference between sample height and the visible surface, including after a region boundary and with varying render rates.

### Choose a hull model

For force-driven interaction, approximate submerged volume with several hull samples and apply the corresponding upward forces at their positions so the hull can pitch and roll. Bound depth and use relative velocity for damping. When the desired behavior is just to follow waves smoothly, compare a filtered target pose derived from surface samples. Keep collision and control ownership explicit in either model.

**Check** release the same vessel at a known tilt, add an off-center load if supported, and send it across waves at several headings. Measure draft, settling time, and peak roll at normal and reduced simulation rates.

### Keep deck contacts in the vessel frame

Represent stable contacts using a deck-relative point, and define how the rider inherits linear and angular motion on departure. Sample the vessel and rider at compatible simulation times. Smooth visual corrections within a budget that keeps collision and presentation aligned.

**Check** walk, stop, jump, land, and disembark across deck edges while the ship pitches. In multiplayer, replay the test with latency and packet loss. Measure deck-relative drift and check that transferred velocity is consistent.

### Tune the helm and camera together

Tune thrust or sailing response separately from buoyancy. Specify how speed changes steering and how a vessel coasts. Choose how much pitch and roll reaches the camera based on comfort and how clearly the player needs to read sea state. Offer a stable-horizon view where useful.

**Check** sail the same route in calm seas and storms from a deck camera and an external view. Compare turning radius, settling time after input release, wave readability, and landing alignment near docks.
