# Game resource handles and registries

## Problem and design reasoning

An authored reference must survive editing and packaging, while a live object may be reloaded, evicted, or replaced. Treating the file name, stored identity, and live pointer as interchangeable creates broken references after moves and stale references after unload.

In his GDC 2005 talk on *Halo 2*, Mat Noguchi describes a persistent object and tag system, including dependency data used to update references when moving groups of tags. Bungie's GDC 2018 *Destiny 2* pipeline talk describes resolving asset references at load time. This let developers package an edited asset and try it alongside packages from the build farm. A practical inference is to separate the identity stored by content from the lookup and access rules of a live registry. These sources document their reference and tooling choices, while the typed handle contract below is a portable design option. [Mat Noguchi, Content Management for Halo 2 and Beyond, GDC 2005](https://nikon.bungie.org/misc/gdc2005_mnoguchi/) · [Brandon Moro, Bungie's Asset Pipeline, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Moro_Brandon_Bungies_Asset_Pipeline.pdf)

## Techniques and practical checks

### Keep identity stable through moves

Persist an identity and expected resource type in references. Resolve that pair through a catalog that maps it to the current package and locator. Use an explicit redirect or migration when a resource is intentionally superseded.

**Check** rename and move a referenced material, rebuild, and load the scene in a clean player build. Confirm the reference resolves. Introduce a duplicate ID and verify that validation identifies both conflicting assets.

### Distinguish durable references from live handles

Let authored data use a durable reference. Let runtime code use a typed handle with validity rules, such as a registry generation for a recyclable slot. If a resource needs to stay alive across a job or frame, require an explicit retained reference for that interval.

**Check** unload a resource, recycle the slot for a different asset, and resolve the old handle. Confirm it fails without returning the replacement and a fresh durable lookup follows the documented policy.

### Give load failure a typed result

Represent pending, ready, missing, wrong type, and failed load distinctly. Allow placeholders only where the caller can keep correct behavior without the asset. Attach the durable ID and requesting context to diagnostics.

**Check** remove a cosmetic texture and a required rules asset from a test package. Confirm the texture uses the intended fallback and the missing rules asset prevents activation with an actionable diagnostic.

### Publish registry changes coherently

Validate catalog entries and dependencies as a batch before making a new revision visible. Decide when existing users may keep the old resource and when they must resolve it again. Release retired resources after their last legitimate use.

**Check** update an asset while a scene transition is loading it. Verify consumers observe a consistent revision, a failed update preserves the previous usable state, and retired instances are released after use ends.

See [Async asset lifetimes](async-asset-lifetimes.md) for load ownership and [GPU resource lifetimes](gpu-resource-lifetimes.md) for hardware completion rules.
