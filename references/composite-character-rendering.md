# Composite character rendering

## Problem and design reasoning

Characters assembled from interchangeable parts need coherent motion and visible seams that hold up under animation. Rendering each part independently can multiply submissions, while merging parts can increase assembly latency and complicate independent deformation. Realtime Worlds explained that APB aimed to keep rendering and memory costs predictable as outfit complexity grew. The transferable inference is to choose a composition strategy around change frequency, part behavior, and the cost of a fully assembled character. [Realtime Worlds, APB: Creating a Powerful Customisation System for a Persistent Online Action Game, GDC 2010, slides 17–24 and 37–41](https://media.gdcvault.com/gdc10/slides/Sciglio_Maurizio_APBCustomisationSystem.pdf)

## Shipped examples

- **Quake III Arena (1999).** The released client source draws separate legs, torso, and head models. It positions the torso on an interpolated tag on the legs and the head on a tag on the torso. This is an example of rigid segment composition with animated attachment points. [id Software, Quake III Arena client source, CG_Player](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_players.c#L2175-L2286)
- **APB: All Points Bulletin (2010).** The game shipped with character customization. In its GDC presentation, Realtime Worlds described combining clothing with the base mesh and composing textures. Clients built the final assets from parameters, and the team discussed frame spikes from time-sliced builds and using a cache to control memory costs. [EA, APB release announcement](https://news.ea.com/press-releases/press-releases-details/2010/EA-Ships-Realtime-Worlds-APB-All-Points-Bulletin/default.aspx) · [Realtime Worlds, GDC 2010, slides 11–24 and 37–41](https://media.gdcvault.com/gdc10/slides/Sciglio_Maurizio_APBCustomisationSystem.pdf)

## Techniques

### Give every part a compatibility contract

Declare skeleton or attachment identifiers, bind transforms, seam ownership, and available detail levels. Share one evaluated pose with parts that deform together. Give independent cloth or face animation an explicit update order and its own compatible inputs.

**Check** swap parts while walking, turning, and holding an extreme pose. Inspect attachments and seams in the first rendered frame after the swap and across detail transitions. Reject mismatches during import and test a known invalid part.

### Choose how parts are submitted

Keep rapidly changed or independently simulated pieces separate. Test merging frequently reused combinations with compatible skinning and render state. Count mesh sections and shader passes as well as meshes, since one combined mesh can still require multiple draws.

**Check** compare separate parts and a merged outfit in a representative crowd. Measure assembly time, peak memory, render thread work, and GPU time with shadows and depth passes enabled.

### Author coverage masks and test deformation

Declare which body regions a garment safely hides. Keep a conservative margin around openings at wrists, neck, and ankles. Author masks for each valid layer combination where needed and verify consistency across detail levels.

**Check** test the outfit against body morph extremes and motions such as a raised arm or deep crouch. Inspect opaque and shadow views for holes and intersections, then verify correct geometry restoration when removing a garment.

### Amortize assembly without publishing stale results

Cache by resolved part IDs, body parameters, material settings, and asset versions. Budget work per frame or queue asynchronous assembly and publish only if the request still matches the actor's current outfit. Display a valid fallback until the requested assets are ready.

**Check** queue several swaps while a character loads, then change its outfit or despawn it before completion. Verify that the latest valid outfit wins, canceled work releases resources, and cold loads stay within the frame budget.
