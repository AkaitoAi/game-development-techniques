# Game asset naming and taxonomy

## Problem and design reasoning

An asset can have a stable identity yet remain difficult for people to find or classify. This guide covers human-readable names, search vocabulary, and classification rules. Asset identity and reference migration remain separate implementation concerns.

Epic's Fortnite modeling guidance recommends descriptive mesh names and numbered variants, using chair examples. Its general asset guidance proposes a type prefix, asset name, descriptor, and optional variant. These documented conventions solve discovery problems, but the exact prefixes are engine conventions to translate into a project's own tools. [Fortnite modeling guidance](https://dev.epicgames.com/documentation/en-us/fortnite/modeling-tips-in-unreal-editor-for-fortnite), [Epic naming guidance](https://dev.epicgames.com/documentation/en-us/unreal-engine/recommended-asset-naming-conventions-in-unreal-engine-projects)

For a shipped production context, Brandon Moro's GDC 2018 **Tools Tutorial Day: Bungie's Asset Pipeline: 'Destiny 2' and Beyond** describes processing content for hundreds of creators across studios, through iteration and shipping. That supports treating discoverability rules as shared production infrastructure. The session description does not establish Bungie's naming grammar. The taxonomy and validation recommendations below are a proposed response to that scale. [Developer talk](https://www.gdcvault.com/play/1025285/Tools-Tutorial-Day-Bungie-s)

## Techniques and practical checks

### Name the object and its meaningful variation

Choose a compact grammar with a semantic noun, relevant role, and variant. Define which distinctions belong in names and which belong in tags. Keep temporary approval status and creator initials out of permanent names unless the team has an explicit reason to retain them.

**Check** Give another contributor ten representative names without opening the assets. Verify they can distinguish intended objects and variants, including meshes, materials, audio, and source files.

### Separate classification dimensions

Use controlled fields for asset type, gameplay role, visual family, platform, and ownership. Permit multiple tags where an asset genuinely serves several roles. Define synonyms so searches for a term such as vehicle also find approved car terminology.

**Check** Search using terms used by design, art, and audio contributors. Confirm each query retrieves the same intended content without requiring knowledge of a particular folder.

### Validate where content enters the project

Validate required fields, permitted characters, case collisions, reserved names, and variant uniqueness at import or submission. Provide the offending asset, the violated rule, and a proposed correction. Begin with warnings when adopting rules in an existing library.

**Check** Submit names differing only by case, a missing noun, duplicate variants, and an invalid character. Verify diagnostics are actionable and valid existing content remains usable.

### Evolve vocabulary without losing references

Version the vocabulary and record deprecated terms. Plan renames with a preview of affected references, source/export pairs, and external tools. Keep identifiers stable through a label change when the pipeline supports it. Test the resulting content build before removing compatibility mappings.

**Check** Rename a shared asset family and reopen a dependent level and source scene. Confirm references survive, search finds the new term, and the old term has a documented migration path.
