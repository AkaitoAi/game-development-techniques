# Mesh instance variation authoring

## Problem and design reasoning

Repeated geometry makes production affordable but exposes duplicated silhouettes, spacing, and attachments. Unrestricted randomization can instead break sockets, collision, readability, or render budgets. This guide concerns authored geometric variation and reproducible placement, rather than material wear or runtime LOD selection.

David Santiago's GDC 2015 talk, *Procedural and Automation Techniques for Design and Production of Sunset Overdrive*, describes curve-driven streets, railways, and ground becoming playable quickly. Designers could evaluate layouts before environment artists refined buildings. The documented reasoning is faster iteration and less custom ground modeling, not randomness for its own sake. [Official session and overview](https://www.gdcvault.com/play/1022216/Procedural-and-Automation-Techniques-for)

The accompanying first-party presentation establishes the production context for those systems. The recipe below extends that iteration principle into a portable mesh-variation workflow. It is a recommendation, not a claim that *Sunset Overdrive* used the exact seed, socket, or batching scheme proposed here. [Santiago's presentation](https://media.gdcvault.com/gdc2015/presentations/Santiago_David_Procedural_and_Automation.pdf)

## Techniques and practical checks

### Define a variation contract

For each family, declare dimensions, pivot, attachment sockets, walkable boundaries, collision envelope, and permitted transformations. Separate silhouette alternatives from optional dressing. Keep landmarks and traversal affordances recognizable across the family.

**Check** Substitute every variant into a representative placement. Verify socket alignment, ground contact, traversal clearance, collision, and the landmark's identity from gameplay distance.

### Generate reproducibly

Derive choices from a stable placement identifier plus an explicit seed. Store intentional overrides separately. Regenerating a neighboring region should not reshuffle approved placements. Version the generator when a changed selection rule could alter saved content.

**Check** Regenerate twice and compare placement identifiers, choices, and transforms. Edit one region and verify that approved neighboring instances remain unchanged.

### Control repetition spatially

Use weighted families, minimum spacing, and neighborhood rejection where repetition is conspicuous. Constrain rotation and scale by meaning. A tree may tolerate broad rotation, while doors and readable signs need orientation rules. Review from player routes instead of judging only a top-down distribution.

**Check** Capture repeated routes at gameplay height and distance. Identify adjacent matching silhouettes, implausible scaling, blocked sightlines, and variation that disguises an interaction.

### Budget the final placements

Retain shared geometry when possible and group compatible render state. Unique deformation or baked variants may increase geometry memory and draw work. Choose between shared instances and unique assets from measured platform cost. Supply an artist override and a way to inspect the generator's decisions.

**Check** Compare a uniform baseline with the varied scene on target hardware. Record geometry memory, draw work, frame time, and edit turnaround, then inspect the most expensive variant cluster.
