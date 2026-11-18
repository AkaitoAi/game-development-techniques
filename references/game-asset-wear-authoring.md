# Game asset wear authoring

## Problem and design reasoning

Uniform edge damage can make every object look equally old while hiding the material beneath it. Wear should communicate how an object was made, used, maintained, and exposed. Naughty Dog's vehicle presentation for *The Last of Us Part II* describes detailed vehicles as important to communicating decay. It also identifies texture allocation and production time as constraints on the previous pipeline. [Matthew Johns, Driving Innovation: A New Vehicle Pipeline, GDC 2021](https://www.gdcvault.com/play/1027001/-The-Last-of-Us)

The *The Last of Us Part I* material-art session discusses procedural generators for architectural materials and combining procedural work with organic sculpting. That establishes a production rationale for reusable material processes. The cause-based wear workflow below is a recommendation for other projects. It does not claim those teams used the exact masks or review fixtures described here. [Jonathan Benainous and Jared Sobotta, The Material Art of The Last of Us Part I, GDC 2023](https://gdcvault.com/play/1029357/Adobe-Developer-Summit-The-Material)

See [Game material authoring validation](game-material-authoring-validation.md) for material inputs and [Trim sheet authoring](trim-sheet-authoring.md) for reusable surfaces.

## Techniques and practical checks

### Describe a material history

Choose the base material, coating, exposure, handling points, and age range. Separate contact abrasion, accumulated dirt, moisture damage, and structural breakage so each has a plausible location and scale.

**Check** show the asset without narrative labels and ask reviewers what caused its most prominent marks. Compare their interpretation with the intended history and inspect any mark that implies an unintended interaction or weak point.

### Use generated masks as editable inputs

Start curvature, ambient occlusion, or directional masks from validated geometry and scale. Adjust masks around protected edges, repeated contact, drainage, and repairs. Expose a few meaningful parameters for related assets.

**Check** compare a flat, convex, and concave asset under the same generator settings. Inspect bevels and UV seams, then verify that manual corrections survive a documented rebake or can be reproduced from saved inputs.

### Preserve the underlying surface response

Give exposed substrate and remaining coating appropriate roughness, color, and normal detail. Review those layers under neutral reference lighting before judging the asset only in its final scene.

**Check** rotate a broad light across the asset and compare close and gameplay distances. Verify coating, damage, and dirt remain distinguishable without crushing values or creating noisy highlights.

### Budget variation across a family

Reuse shared material logic while varying damage placement and age where players can notice repetition. Keep the number of unique textures or masks within the project's residency and authoring budgets.

**Check** assemble several copies in the intended scene. Measure texture memory and material cost, inspect repeated damage at gameplay distance, and confirm the most important silhouettes and interaction cues remain clear.
