# Release engineering and patch delivery

## Problem and design reasoning

**Problem.** A patch works on a clean install but fails on common older builds, or a rollout leaves clients and servers on incompatible versions. Count affected upgrade paths and record the last recoverable state.

**Design reasoning.** Wube's stable and experimental distribution channels show a documented split between broad availability and ongoing testing. An inferred trade-off is to expand availability only once the actual update paths, save compatibility, and recovery options have been exercised. See the shipped examples below for sources.

## Shipped examples

**Doom (1993).** In the released Linux source, a recorded demo carries a version byte and the playback path checks it before initializing the game. Even a compact format needs an explicit compatibility policy when the executable and recorded data can change independently. [id Software demo code](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/g_game.c)

**Factorio (2020 onward).** Wube distributes stable and experimental builds and offers an automatic updater for direct-download installations. Steam installations use Steam's distribution tools. The same project therefore has distinct release channels and more than one delivery path. A release checklist needs to cover each supported path and distinguish a tested candidate from the default build. [Wube release downloads](https://www.factorio.com/download) · [Official Factorio installation guide](https://wiki.factorio.com/Install_guide)

## Techniques and practical checks

### Identify the release artifact

Record the source revision, content manifest, target, build configuration, toolchain version, and output digest. Promote the same validated artifact between channels when a store allows it. When store processing changes the package, verify the final installed build through that channel.

**Check** a build by starting from its release record, then match what the tester installs to the expected revision and content. Confirm that crash reports can be matched to the right symbols.

### Validate patch paths and sizes

Keep package boundaries stable where possible. Track bytes downloaded and temporary space used when an asset changes. Test updates from the oldest supported base and commonly installed bases as well as fresh installs. Use artifact integrity checks and a supported full-package recovery path.

**Check** one changed texture and one changed shared dependency. Measure transferred bytes and disk requirements. Interrupt installation and verify that retry or recovery produces the same installed content as a fresh install.

### Stage rollout and define recovery

Set criteria for promoting a candidate and for halting distribution. Track launch failures, update completion, save-load errors, and service compatibility by build and platform. Before release, decide which changes can be rolled back and which need a forward repair, especially changes to persistent or shared data.

**Check** a rehearsal where the candidate fails the launch threshold. Stop promotion, recover using the documented path, and test both an untouched save and one already written by the candidate.

### Plan for version skew

Make client, server, save, downloadable content, and replay compatibility explicit. Define the minimum supported versions and an out-of-date message for each mismatch players can see. Sequence releases around dependencies that may arrive at different times.

**Check** old and new clients against old and new services where supported. Also try offline launch, a pending platform update, and a partially installed optional content package. Verify that progress is preserved and that a blocked player gets a clear recovery action.
