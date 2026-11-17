# Water rendering

## Problem and design reasoning

Water must read as a surface with depth, motion, and scale. A single scrolling normal map can work for a distant pond, but it cannot provide the moving silhouette of a nearby swell or describe the current around a rock. Naughty Dog chose to break rapids into artist-controllable procedural components and use offline simulation to inform shape and flow. Rare used a spectral ocean simulation while applying stylized color rules to the result. A transferable inference is to spend runtime work on the motion the player can judge from the current viewpoint, then use simpler shading or cached data for the rest. [Naughty Dog, Rendering Rapids in Uncharted 4, SIGGRAPH 2016](https://advances.realtimerendering.com/s2016/index.html) · [Rare, The Technical Art of Sea of Thieves, SIGGRAPH 2018](https://history.siggraph.org/wp-content/uploads/2022/09/2018-Talks-Ang_The-Technical-Art-of-Sea-of-Thieves.pdf)

## Shipped examples

- **Uncharted 4: A Thief's End (2016).** The water system handled a range of water bodies. To represent rapids, the team used offline fluid simulations to produce surface and flow data and split the runtime motion into geometric and shading components. [Carlos Gonzalez-Ochoa, Rendering Rapids in Uncharted 4, SIGGRAPH 2016](https://advances.realtimerendering.com/s2016/index.html)
- **Sea of Thieves (2018).** Rare used an FFT-based ocean simulation and blended deep and subsurface colors using view angle, sun direction, and a wave-peak mask. The talk presents these operations as part of giving the open world a stylized look. [Rare, The Technical Art of Sea of Thieves, SIGGRAPH 2018](https://history.siggraph.org/wp-content/uploads/2022/09/2018-Talks-Ang_The-Technical-Art-of-Sea-of-Thieves.pdf)

## Techniques and practical checks

### Split displacement from shading detail

Use geometry for wave scales that change the horizon, shore contact, or nearby object silhouettes. Use normal detail for smaller ripples and fade it with distance. If objects respond to a sampled water height, identify which wave bands drive that height and keep their phase consistent with the visible surface.

**Check** from the shore, at surface height, and from an elevated moving camera. Observe silhouettes and floating objects through mesh detail transitions. Record any seam, visible height mismatch, or displacement discontinuity.

### Author directional flow and localized foam

Represent river flow as a direction and speed field sampled in world space or along an authored channel. Drive fine surface motion with the field and derive foam from explicit shoreline, obstacle, wake, or wave-crest signals. Give each foam source a fade or lifetime that matches its cause.

**Check** a bend, a rock near the bank, and a section where two flow directions meet. Trace a patch of foam and confirm that apparent motion remains downstream, with no repeated texture jump or swimming across static geometry.

### Make reflection and depth failure modes visible

Use view-angle weighting for reflection and water depth for attenuation or color. Where screen-space reflections or refraction lose off-screen content, define a fallback and guard against sampling foreground geometry. Establish how water appears from below if the camera can cross the surface.

**Check** the same pool at grazing and steep angles, then move a reflected object off-screen and pass through the waterline. Inspect bank halos, disappearing reflections, and underwater exposure. Repeat with expensive reflections disabled.

### Budget the hardest water shot

Capture displacement, lighting, reflection, and transparency cost individually. Set quality tiers for wave resolution, reflection quality, and overdraw. Where complex flow is authored offline, validate that cached inputs remain in sync with level geometry.

**Check** a full-screen close view with overlapping foam, dynamic lighting, and moving actors. Measure GPU pass time and visual stability on the lowest supported tier. Change a marker rock and confirm the affected offline flow can be rebuilt or flagged for review.
