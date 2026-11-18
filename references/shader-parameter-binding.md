# Shader parameter binding

## Problem and design reasoning

A material may contain the right values while a draw reads the wrong buffer, layout, or resource slot. Parallel rendering makes stale or overwritten bindings harder to reproduce. Natalya Tatarchuk's *Destiny* GDC 2015 talk describes encapsulating synchronization and coherent rendering data so graphics engineers can use a manageable API. That supports treating the draw's inputs as an explicit contract. The layout and lifetime protocol below is a portable recommendation. [Bungie, Destiny's Multi-threaded Renderer Architecture, GDC 2015](https://advances.realtimerendering.com/destiny/gdc_2015/)

Microsoft documents that a Direct3D 12 root signature must agree with shader resource declarations. Other graphics APIs express related constraints through their own layouts and bindings. Record those constraints in engine metadata and validate them when constructing a pipeline. [Microsoft, Resource binding in HLSL](https://learn.microsoft.com/en-us/windows/win32/direct3d12/resource-binding-in-hlsl)

## Techniques and practical checks

- Generate or validate a binding schema from compiled shader metadata. Include resource type, array length, constant offsets, and shader variant. **Check** a deliberately wrong texture type, missing constant, and array count mismatch. Require a useful diagnostic before submission.
- Separate frame, view, material, and object inputs by their update frequency. Snapshot the values required by a submitted draw. **Check** two views drawing the same material while an object parameter changes. Inspect which values each draw actually reads.
- Keep uploaded constants and descriptor references alive until their consumers finish. Recycle transient storage through the renderer's completion mechanism. **Check** several frames in flight with forced GPU delay and rapid parameter changes. Look for stale values and premature reuse.
- Make a fallback binding explicit for unavailable optional resources. Cache bindings only while the schema and resource identities remain valid. **Check** shader reload, texture replacement, and resource eviction. Compare the resulting draw with a fresh uncached binding.

For author-facing parameter ownership, see [Runtime material parameter management](runtime-material-parameter-management.md).
