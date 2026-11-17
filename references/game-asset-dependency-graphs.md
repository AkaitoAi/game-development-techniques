# Game asset dependency graphs

## Problem and design reasoning

An asset reference can control what rebuilds, what ships, and what must load before an object is usable. Mixing those relationships makes a small change dirty large parts of the game or lets a build succeed with missing runtime content. Bungie's *Destiny 2* presentation describes a scalable graph-based processing pipeline and explains how its predecessor evolved to support the sequel. [Brandon Moro, Bungie's Asset Pipeline: Destiny 2 and Beyond, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Moro_Brandon_Bungies_Asset_Pipeline.pdf)

For *God of War Ragnarök*, Santa Monica Studio used dependency scans to populate an asset database for visual scripting. The developer presentation reports that this supported safer runtime lookups and better authoring feedback, while coupling the database to the build introduced an expensive cycle. A transferable inference is to represent authoring references, build inputs, and runtime prerequisites as distinct relationships, then validate each relationship under its own rules. [Samuel Sternklar, Santa Monica Studio, GDC 2023](https://media.gdcvault.com/gdc2023/Slides/God%2Bof%2BWar_Sternklar_Samuel%2B2023-03-31%2B07.08.17.pdf)

## Techniques and practical checks

### Give each edge a meaning

Represent dependencies with stable asset IDs, a relationship type, and platform or variant constraints when applicable. Record which authoring field created the edge. Keep a required load prerequisite distinguishable from an optional lookup and from an importer-only input.

**Check** move a referenced asset, delete it, and change it to an incompatible type. Verify that move behavior follows the ID policy and errors identify the originating asset and field.

### Compute invalidation from declared inputs

Include source content, importer version, settings, and target in the inputs that can invalidate a derived artifact. Follow reverse build edges when those inputs change. Explain invalidation through the dependency path so a team can distinguish a justified rebuild from a hidden global dependency.

**Check** edit an asset used by one feature and a shared source used by several, then alter an importer setting. Compare incremental results with a clean build of the same inputs and inspect why each artifact rebuilt.

### Reject cycles where the relationship requires an order

Build transformations need a valid ordering of prerequisite work. Runtime references may permit cycles, depending on the loader. For a permitted cycle, resolve the group without recursive blocking and define when its members become usable. Treat generated metadata as its own dependency if authoring or builds consume it.

**Check** introduce a build cycle and a permitted runtime reference cycle in test content. Verify the build fails with a readable cycle path and the runtime case reaches a complete, usable state.

### Verify closures for shipping configurations

Walk hard runtime dependencies from each shipping root and check package or streaming availability against the target platform. Give dynamic lookups a manifest or validation rule so they can be exercised outside the authoring environment. Use edge types to identify assets included solely because an optional relationship was marked required.

**Check** boot a packaged build without developer content. Exercise an optional pack and a platform-specific variant, then simulate a missing required dependency in a test build and confirm the expected diagnostic or fallback.
