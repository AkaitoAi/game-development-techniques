# Derived asset build caches

## Problem and design reasoning

Texture encoding, mesh processing, and other asset transformations can dominate iteration when editors and build workers repeat equivalent work. A cache can also make builds quietly stale when its key omits a dependency or target setting. Treat each cache lookup as a request for the result of a specific, repeatable transformation.

- At GDC 2019, Pete Kugler described the build and data-cache improvements Bungie made to support small teams collaborating on feature branches for *Destiny*. [Pipeline Support for Feature Branches in Destiny](https://gdcvault.com/play/1025992/Pipeline-Support-for-Feature-Branches)
- Epic documents an industry implementation in which cached derived data is disposable and regenerated from authored assets. Its cloud cache documentation describes using hashes to identify stored payloads. [Epic's Derived Data Cache guide](https://dev.epicgames.com/documentation/unreal-engine/using-derived-data-cache-in-unreal-engine?lang=en-US) · [Unreal Cloud DDC](https://dev.epicgames.com/documentation/en-us/unreal-engine/how-to-set-up-a-cloud-type-derived-data-cache-for-unreal-engine)

The architecture below follows from those constraints. An action key identifies the inputs and environment of a transformation, while a content digest identifies the produced bytes. Several action keys may point to identical output bytes.

## Implementation and checks

- Include source content, dependency content, relevant import options, target platform and format, and the transformation version in the action key. Record the transformation's dependencies even when it discovers them during execution. **Check** change a shared material dependency, an import setting, and a compressor version separately. Confirm that each change invalidates the affected variants and preserves unrelated cache hits.
- Write intermediate results in temporary storage, verify output size and digest, and publish the action-to-output record only after all outputs are complete. **Check** interrupt a worker during a write, then run two workers on the same key. Readers should see a valid complete entry or a miss.
- Keep the cache optional and measure retrieval cost. Use local hits for quick iteration and shared hits when transfer is cheaper than recomputation. Define what happens on timeouts or failed checksum validation. **Check** compare representative cold, warm local, and warm shared builds. Simulate an unavailable cache and confirm the build still completes with equivalent outputs.
- Version cache formats separately from runtime package formats and preserve authored assets and their build recipes as the rebuildable source. **Check** rebuild the same revision on clean workers from two branches with identical effective inputs. Compare output digests, or compare explicitly normalized content if the format embeds expected nondeterministic metadata. Repeat with a platform setting changed and verify the result stays isolated.
