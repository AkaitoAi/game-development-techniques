# Runtime mesh generation

## Problem and design reasoning

Generated geometry must have a clear owner and become visible only when its buffers and dependent data are ready. Otherwise a cancelled request can publish an obsolete surface or leave rendering and collision describing different versions of the world.

The released OpenGL renderer for *Quake* subdivides water and sky polygons, creates linked polygon data, and computes texture coordinates for later drawing. Its source comments explain that subdivision enables the warp effects. This is an example of preparing reusable geometry at load time and animating its appearance during play. [id Software, Quake surface subdivision](https://github.com/id-Software/Quake/blob/master/WinQuake/gl_warp.c)

Innes McKendrick's GDC 2017 talk on *No Man's Sky* describes a continuous generation pipeline from voxel data through polygonization, texturing, population, and simulation. Hello Games needed this pipeline to support seamless traversal from space to populated terrain with a small team. The following portable design recommendations are inferred from that ownership and continuity problem. [Hello Games, Continuous World Generation in No Man's Sky](https://www.gdcvault.com/play/1024265/Continuous-World-Generation-in-Noby)

## Techniques and practical checks

### Version requests and publish complete results

Give each generation request an owner, a source-data revision, and a cancellation state. Generate into private working storage. At publication, compare the result's revision with the current owner before swapping the visible mesh. Retire replaced GPU buffers only after in-flight work has completed.

**Check** issue two requests for one region, delay the first, and unload the owner before either completes. Verify the newer result wins while the owner exists and neither result publishes after unload.

### Specify geometry invariants

Define vertex attributes, index width, winding, coordinate space, and bounds in the generator's output contract. Validate finite attribute values and indices before upload. When adjacent pieces share a border, derive border positions and normals from a consistent rule.

**Check** empty output, maximum supported vertex count, degenerate input, and neighboring chunks generated in opposite orders. Inspect normals, culling, bounds, and seams while moving the camera.

### Coordinate visual and collision versions

Choose when generated collision becomes authoritative. Publish the visual and collision revisions together when gameplay depends on an exact match, or make their temporary mismatch an explicit bounded state. Preserve a usable collision representation while replacement work runs.

**Check** edit the surface beneath a moving character and delay collision generation. Verify the character follows the defined policy and no obsolete collider reappears after a newer revision commits.

### Bound generation and upload work separately

Measure source sampling, polygonization, attribute construction, collision building, and GPU upload independently. Limit concurrency and upload bytes per frame. Reuse working buffers only after the previous job releases them.

**Check** rapidly cross generation boundaries and repeatedly reverse direction. Record queue depth, generation age, peak memory, upload cost, and frame-time tails. Verify cancellation reduces obsolete work and that the playable region remains covered.
