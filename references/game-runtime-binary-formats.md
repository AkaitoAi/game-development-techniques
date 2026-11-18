# Game runtime binary formats

## Problem and design reasoning

Runtime asset bytes cross a boundary between the content builder and the executable. A layout mismatch can turn an ordinary load into incorrect offsets, excessive allocation, or broken references. The released *Quake III Arena* MD3 format defines an identifier, version, counts, and offsets, and its loader checks the version and converts byte order. These are shipped examples of an explicit runtime format, rather than proof of a comprehensive malformed-file validator. [id Software, format declarations](https://github.com/id-Software/Quake-III-Arena/blob/master/code/qcommon/qfiles.h) · [id Software, model loading](https://github.com/id-Software/Quake-III-Arena/blob/master/code/renderer/tr_model.c)

Bungie's GDC presentation describes game packages for *Destiny* that avoided runtime reference fixup at the cost of processing mutually referencing data together. For *Destiny 2*, load-time asset reference resolution supported smaller build graphs and faster iteration. This illustrates a format trade-off between load work and content-build coupling. The validation and publication protocols below are portable recommendations. [Brandon Moro, Bungie's Asset Pipeline: Destiny 2 and Beyond, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Moro_Brandon_Bungies_Asset_Pipeline.pdf)

## Techniques and practical checks

### Specify the reader-writer contract

Record format identity, supported version, byte order, field widths, alignment, and target requirements. Define optional chunks and the policy for unknown versions. Serialize declared fields rather than depending on an incidental native structure layout.

**Check** build one representative asset with each supported writer version and read it with the intended runtime. Include an unsupported version and a target mismatch. Verify a clear load failure before the asset becomes visible.

### Validate ranges before interpreting payloads

Check counts, offsets, lengths, alignment, and required chunk relationships against the actual buffer size. Perform overflow-safe range arithmetic before allocation or pointer construction. Bound decoded size and verify indices and finite numeric values where the asset contract requires them.

**Check** a truncated header, a payload ending one byte early, a count-times-stride overflow, an out-of-range index, and an invalid alignment. Require controlled rejection with bounded allocation and no published partial asset.

### Resolve references in a staged load

Separate validated bytes from a ready runtime resource. Resolve stable asset identifiers through the registry and define whether missing optional dependencies produce fallback content or a failed load. Retain source buffers for readers that refer directly into them.

**Check** load two mutually referencing assets in either completion order, remove an optional dependency, and cancel during fixup. Verify terminal results, reference ownership, and cleanup before retrying.

### Keep format changes tied to build identity

Include relevant format and converter versions in derived-asset cache keys. Preserve an inspectable content manifest and useful diagnostics naming the asset and failing field. Choose an explicit recook or compatibility path when layouts change.

**Check** change a field layout or converter version, then compare incremental and clean builds. Load an old cached artifact deliberately and verify it is rejected or upgraded through the supported path. Confirm a repeated failed load does not accumulate resources.

See [Derived asset build caches](derived-asset-build-caches.md) and [Game resource handles and registries](game-resource-handles-and-registries.md) for related ownership and identity contracts.
