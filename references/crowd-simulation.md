# Crowd simulation

## Problem and design reasoning

A dense crowd needs to move coherently, react near the player, and stay within simulation and rendering budgets. Individual agents need enough fidelity to support the scene's interactions. IO Interactive explicitly prioritized animation quality because players pass close to individual crowd members. Ubisoft described recycling expensive AI and high-detail models across a much larger crowd in selected scenes. The transferable inference is to budget individual fidelity according to what players can see and affect. [IO Interactive, Crowds in Hitman: Absolution, GDC 2012](https://media.gdcvault.com/gdc2012/slides/Programming%20Track/Fauerby_Kasper_CrowdsInHitman.pdf) · [Ubisoft, Massive Crowd on Assassin’s Creed Unity: AI Recycling, GDC 2015](https://www.youtube.com/watch?v=Rz2cNWVLncI)

## Shipped examples

- **Hitman: Absolution (2012).** IO Interactive partitioned crowd space into cells containing agent occupancy and metadata such as ambient flow and exclusion areas. The team moved from simpler shared loops to individually blended visible poses driven by motion clips. The developers reported better motion quality and slower response to steering input, which made intersections harder to prevent. [IO Interactive, Crowds in Hitman: Absolution, GDC 2012](https://media.gdcvault.com/gdc2012/slides/Programming%20Track/Fauerby_Kasper_CrowdsInHitman.pdf)
- **Assassin’s Creed Unity (2014).** Ubisoft presented separate work on scaling the visible crowd through AI recycling and making crowd events provide spontaneous opportunities for the player. Keep gameplay participation explicit when using reduced simulation tiers for background agents. [Ubisoft, Massive Crowd on Assassin’s Creed Unity: AI Recycling, GDC 2015](https://www.youtube.com/watch?v=Rz2cNWVLncI) · [Ubisoft, Developing Systemic Crowd Events, GDC 2015](https://gdcvault.com/play/1022212/Postmortem-Developing-Systemic-Crowd-Events)

## Techniques

### Bound local queries

Assign agents to spatial cells and look up neighbors in overlapping cells. Size queries by sensing radius and predicted movement. Keep route flow, local avoidance, and physical constraints separately tunable.

**Check** two streams entering a narrow doorway. Measure worst-case neighbor queries, overlap duration, exit rate, and the CPU time of the densest cells. Repeat with blocked exits.

### Promote agents deliberately

Give agents a cheap ambient state and promote them when nearby player actions or gameplay commitments require detail. Preserve stable identity, position, and active event state during the handoff. Use distinct promotion and demotion thresholds to avoid rapid switching.

**Check** cross the tier boundary repeatedly while tracking one visible agent and one active participant. Limit the promotion budget and verify stable identity, continuous movement, and a bounded cost when many agents request promotion.

### Tune locomotion and steering together

Feed steering intent into the motion controller and measure its achievable turning and stopping distance. Reserve enough room for that response. Vary animation start phases and select different clips where repeated movement looks synchronized.

**Check** record a dense stream turning around a corner. Count body intersections, compare requested and actual turn radii, and inspect contact foot drift and synchronized steps.

### Give crowd events a lifecycle

Define which agents qualify, how a scene reserves them, and how it releases them after completion or cancellation. Reserve gameplay capacity for active events and prioritize them over ambient density. Add a fallback when the requested participants are unavailable.

**Check** trigger two nearby events, interrupt one halfway, and leave and re-enter the zone. Confirm agents are assigned once, reservations are released, and visible events remain understandable under different crowd densities.
