# Mesh deformation budgeting

## Problem and design reasoning

A character can meet a triangle budget and still exceed its frame budget because skinning, facial shapes, corrective poses, and extra render passes repeatedly deform the same vertices. The authoring problem is deciding which deformation work is visible and which gameplay consumers depend on it.

Naughty Dog's *Uncharted 2* character-pipeline talk documents maintaining shared skeletons across outfits and producing varied meshes while preserving a common pipeline. Wasim Khan's *The Last of Us Part II* presentation covers character authoring and optimization within an iteration-heavy production. These establish shipped constraints and pipeline context. The cost ledger and tier validation below are recommended methods, rather than reported implementations of those games. [Richard Diamant and Judd Simantov, GDC 2010](https://www.gdcvault.com/play/1012798/Uncharted-2-Character-Pipeline-An) · [Wasim Khan, GDC 2021](https://www.gdcvault.com/play/1027376/Character-Technology-of-The-Last)

## Techniques and practical checks

### Budget each deformation stage

Record vertices, influence counts, active morph targets, corrective passes, and additional geometry such as hair or clothing. Measure CPU and GPU costs in a representative build and include shadow or secondary-view consumers.

**Check** capture one actor, the expected crowd, and the maximum supported visible set. Disable one stage at a time and compare its measured cost with the complete frame.

### Reduce work around visible error

Choose influence limits, corrective coverage, and facial detail according to camera distance and pose demands. Evaluate loss in difficult poses, not only the bind pose.

**Check** compare full and reduced variants during a shoulder raise, deep crouch, facial extreme, and equipment interaction at shipping camera distances. Record silhouette, contact, and shading errors.

### Keep dependent consumers valid

Identify sockets, collision shapes, cloth anchors, and gameplay traces that need current bones. A cheaper presentation tier should retain or explicitly replace the data those systems require.

**Check** force each tier while an attachment, contact effect, and gameplay interaction use the affected bones. Verify their transforms and event timing against full evaluation.

### Bound promotion and residency costs

Treat changing deformation quality as a workload transition. Account for pose initialization, buffer residency, and simultaneous promotions after camera cuts or teleports.

**Check** reveal the maximum actor group from off-screen and force a quality change. Measure peak memory, promotion delay, and tail frame time alongside visible pose continuity.

See [Character topology for deformation](character-topology-for-deformation.md) and [Animation runtime LOD](animation-runtime-lod.md).
