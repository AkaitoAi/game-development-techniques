# Mesh pivot authoring

## Problem and design reasoning

A mesh can meet its size specification yet rotate around the wrong point, snap with an offset, or detach from another object after reimport. Its pivot defines the local frame that downstream tools and runtime systems use. Scale establishes dimensions, and socket definitions establish connection points. Pivot authoring determines how an asset behaves when transformed.

In the released *Quake III Arena* client, tag attachment computes a child origin and axes from the parent and an interpolated tag transform. That shipped code illustrates why local frames must be consistent. It does not prescribe a DCC export convention. [id Software, CG_PositionEntityOnTag](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_ents.c)

Bethesda's GDC talk on *Fallout 4* describes modular production for a large world. Predictable placement is an implementation inference for such reusable kits. Epic's FBX documentation explicitly explains its exporter-origin convention and recommends a corner origin for grid placement. Apply that convention only through an import path that supports it. [Joel Burgess and Nathan Purkeypile, Fallout 4's Modular Level Design, GDC 2016](https://www.gdcvault.com/play/1022930/-Fallout-4-s-Modular) · [Epic Games, FBX Static Mesh Pipeline](https://dev.epicgames.com/documentation/en-us/unreal-engine/fbx-static-mesh-pipeline-in-unreal-engine)

## Techniques and practical checks

### Choose a pivot from the asset's operation

Use a documented local frame for placement, rotation, and scaling. A hinged part needs its rotation axis at the hinge. A grid module needs its placement point on the kit's chosen lattice. For assets serving both purposes, use an explicit parent or authoring anchor instead of silently changing the mesh origin.

**Check** place the asset at identity, rotate it through its supported angles, and snap several instances together. Measure the expected hinge or connection point throughout the motion.

### Verify the exported local frame

Record forward and up axes, pivot position, unit conversion, and whether transforms are baked or retained. Inspect the imported transform and geometry bounds independently. Avoid compensating in several hierarchy levels without identifying the owner of the correction.

**Check** export a labeled asymmetric fixture with an off-center hinge. Reimport it through every supported tool path and compare its local axes and transformed reference points.

### Propagate pivot edits through dependents

A pivot edit can change collision placement, animation curves, attachments, and authored scene instances. Define whether reimport preserves the old visible pose or adopts the new authored frame, and migrate affected offsets deliberately.

**Check** move a pivot in a disposable asset with a collider, attached effect, and animated child. Reimport and verify each dependent remains aligned under the chosen migration policy.

### Handle hierarchy and scale cases

Test a parent rotation, nonuniform scale, and mirrored instance where supported. A local pivot that works at identity may expose assumptions in collision or attachment code under those transforms.

**Check** assemble the fixture beneath transformed parents, animate its hinge, and compare attachment and collision positions with known reference points in a player build.

See [Game asset scale conventions](game-asset-scale-conventions.md), [Runtime socket validation](runtime-socket-validation.md), and [Game transform hierarchies](game-transform-hierarchies.md).
