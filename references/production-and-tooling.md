# Game production and tooling

## Problem and design reasoning

**Problem.** Content changes take too long to see in game, or a tool saves authoring time while creating more cleanup. Measure the complete edit, build, review, and correction cycle.

**Design reasoning.** Insomniac's account describes measuring procedural tool value alongside cleanup cost and retaining per-instance controls. Quake's staged tools illustrate a complementary approach to shortening iteration by rerunning the work required for a change. See the shipped examples below for sources.

## Shipped examples

**Quake (1996).** id Software's released build instructions illustrate a staged asset pipeline. The level editor could invoke separate tools for geometry processing, lighting, and visibility, and the same tools could run from the command line. The full build documentation called out the high cost of rebuilding all maps. A map compiler remains a useful model for moving predictable work out of runtime and supporting different iteration speeds during authoring and release. [id Software, Quake tools build instructions](https://github.com/id-Software/Quake-Tools/blob/master/qutils/README.TXT)

**Marvel's Spider-Man 2 (2023).** Insomniac used procedural authoring tools for city content and gameplay markup, including placement of swing-hint volumes and ledge paths. The studio aimed to give artists and designers a way to adjust individual instances. Its retrospective also describes a building resize tool whose cleanup cost undermined its value compared with existing tools. Measure the whole authoring task, including corrections, when evaluating automation. [David Santiago, Insomniac Games, GDC 2024 slides and speaker notes](https://media.gdcvault.com/gdc2024/Slides/GDC%2Bslide%2Bpresentations/Santiago_David_ProceduralToolDev_with_notes.pdf)

**Hades II (2025).** Supergiant used a focused pre-release technical test to surface technical issues, then continued development through Early Access before releasing 1.0. The studio planned Early Access around a build far enough along to gather meaningful feedback and early enough to act on it. Each milestone therefore supported different decisions about the same game. [Supergiant technical test](https://www.supergiantgames.com/blog/hades2-tech-test-sign-up/) · [Supergiant development update](https://www.supergiantgames.com/blog/hades-ii-development-update/) · [Supergiant 1.0 launch](https://www.supergiantgames.com/blog/hades2-now-available/)

The cases show tooling at three scales. The build pipeline turns authored inputs into runtime data, the editor helps content teams produce and revise individual instances, and staged releases bring player feedback into production decisions. Each reduces a different delay between making a change and seeing its effect.

## Techniques and practical checks

### Separate source assets from generated outputs

Keep editable assets and their import settings in version control. Declare each build stage's inputs and outputs so an incremental build can invalidate only affected products. Include dependency changes, platform settings, and tool versions in invalidation. Expose the same processing steps from the editor and the command line. Provide a lightweight preview when expensive processing slows routine edits.

**Check** by building a representative scene from a clean checkout, then changing a shared texture, a map, and a tool version separately. Verify the expected outputs rebuild and that the packaged scene matches a clean build of the changed inputs. Compare semantic output where a format contains timestamps or other incidental metadata.

### Make procedural authoring editable

Start with a high-frequency task and a small, representative sample. Store generation parameters and an explicit seed where randomness is involved. Assign stable identities to generated items and store authored overrides against those identities. Preview the changes and report overrides whose targets disappeared. Give the author a deliberate operation for baking or discarding generated work.

**Check** by generating a sample area twice, editing one generated instance, and changing the source layout. Verify that unchanged instances keep their identity, overrides survive where their targets still exist, and deleted targets are reported. Compare the full time and defect rate for manual and assisted authoring, including review and cleanup.

### Put validation at the point of change

Have asset checks report the asset, failing rule, and correction. Run inexpensive checks at import or save, then run cross-asset checks in a repeatable packaged build. Give collaborators a way to reproduce the same validation locally with the same settings. Use a small packaged smoke test for failures that are invisible in the editor.

**Check** by introducing a missing reference, a duplicate ID, and a platform-specific asset problem in a test fixture. Confirm each is reported at the intended stage with a location and an actionable message. Install the packaged build on a representative device and verify that it reaches playable content.

### Match playtest builds to specific decisions

Write the decision the next build must inform, identify the players and hardware needed, and choose a scenario or telemetry signal to answer it. Label reports with build ID, platform, starting state, and reproduction steps. When a build carries persistent progression, test save upgrades using copies of representative previous-version saves.

**Check** by reproducing a reported issue from a clean install or known save, then rerunning that scenario on the candidate fix. Verify that players can complete the intended loop and that the build answers the milestone's question. Track issues that require a separate balancing or technical test.
