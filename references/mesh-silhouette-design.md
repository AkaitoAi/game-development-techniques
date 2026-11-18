# Mesh silhouette design

## Problem and design reasoning

A detailed model can become unreadable at the distance where players must identify it. Valve's *Illustrative Rendering in Team Fortress 2* describes art and engineering working together to communicate geometry and support player identification under varied lighting. Distinctive silhouettes and internal shape treatments serve gameplay readability. [Jason Mitchell, Moby Francke, and Dhabih Eng, NPAR paper](https://cdn.akamai.steamstatic.com/apps/valve/2007/NPAR07_IllustrativeRenderingInTeamFortress2.pdf)

Valve also lists Jason Mitchell's GDC presentation *Stylization With a Purpose: The Illustrative World of Team Fortress 2*. This documented approach connects visual style to gameplay requirements. The checks below extend that principle into an engine-agnostic modeling workflow; their thresholds should be set for the project's camera and decision timing. [Valve conference publications](https://www.valvesoftware.com/en/publications)

## Techniques and practical checks

### Set the recognition distance

Record the smallest expected screen size and the decisions supported by shape, such as enemy role, equipment, or interactable function. Block primary masses before adding surface detail. Compare candidates at gameplay scale throughout modeling.

**Check** display solid silhouettes at target screen sizes for a brief, repeatable interval. Ask testers to identify roles and record which pairs they confuse.

### Protect identifying shapes

Assign a few clear features to each role through proportions, posture, and equipment placement. Preserve those features through costume variants and level-of-detail reductions. Keep ornamental additions from overwhelming the main masses.

**Check** compare the base model, variants, and every shipped detail level from relevant camera angles. Confirm the features needed for recognition survive transitions.

### Author negative space through motion

Inspect gaps between arms, torso, legs, and equipment. An informative neutral pose can collapse into one shape during aiming, crouching, or attack anticipation. Review meshes with representative animations early.

**Check** sample idle, locomotion, attack, and recovery at multiple view angles. Flag merged shapes that obscure the action or role when a player must respond.

### Validate in the scene

Test recognition against representative environments, crowds, partial occlusion, and lighting. Use silhouette renders to diagnose geometry, then repeat with shipping materials and effects to assess the complete image.

**Check** measure recognition accuracy and response time in quiet and crowded encounters. Rework recurring confusions and repeat the same captures to compare changes.
