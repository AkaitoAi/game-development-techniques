# Build automation

## Problem and design reasoning

Large projects need reproducible runnable builds without making development wait for every expensive job. Rare describes using continuous integration workers to build and test Sea of Thieves, and Bungie describes changing its build and caching infrastructure to support parallel feature development. A practical inference is to make the input revision and dependency set explicit, then shorten feedback on common changes while retaining broader validation for milestone builds. [Rare, GDC 2019](https://media.gdcvault.com/gdc2019/presentations/Masella_Robert_AutomatedTestingOf.pdf) · [Bungie, Pipeline Support for Feature Branches in Destiny, GDC 2019](https://www.gdcvault.com/play/1025992/Pipeline-Support-for-Feature-Branches)

## Shipped examples

**Sea of Thieves (2018).** Rare's testing presentation describes a TeamCity build farm that dispatches jobs such as compiling the game and running automated tests. Its multiplayer AI tests exercise behavior that requires several processes to cooperate. The build system provides a repeatable environment for these checks. [Robert Masella, Automated Testing for Multiplayer Game-AI in Sea of Thieves, GDC 2019](https://media.gdcvault.com/gdc2019/presentations/Masella_Robert_AutomatedTestingOf.pdf)

**Destiny 2 (2017 onward).** Bungie describes adapting build and data caching infrastructure as feature-branch use scaled. Its CEDEC session discusses organizing branching strategy, roles, and software to sustain collaboration on a live game. Teams developing features in parallel still need consistent inputs and visible integration results. [Bungie, Pipeline Support for Feature Branches in Destiny, GDC 2019](https://www.gdcvault.com/play/1025992/Pipeline-Support-for-Feature-Branches) · [Bungie, Collaboration While Managing Stability on a Live Service Game, CEDEC 2022](https://cedec.cesa.or.jp/2022/session/detail/211.html)

**Call of Duty: Modern Warfare II (2022).** Microsoft and Activision describe using build timing data to locate expensive compilation work. Their account connects slow builds to delayed verification in continuous integration. [Microsoft and Activision, Build Insights presentation write-up, GDC 2025](https://developer.microsoft.com/en-us/games/articles/2025/03/gdc-2025-build-insights-call-of-duty-modern-warfare/)

## Techniques and practical checks

### Declare build inputs and identify outputs

Record the source and content revision, toolchain version, platform, target, and feature flags. Make build commands usable on both local machines and workers. Attach the input manifest and logs to each artifact.

**Check** by building the same revision on two clean workers. Compare version metadata, asset manifests, and launch behavior. Where the artifact format supports reproducibility, compare digests.

### Keep cache invalidation testable

Key reusable output by the inputs that can change its semantic result, including asset dependencies and importer versions. Keep a clean-build path for confirming incremental output.

**Check** an asset edit, deletion, tool upgrade, and branch switch with a warm cache. Compare affected outputs against a clean build and account for expected nondeterministic metadata.

### Separate fast and expensive feedback

Run inexpensive validations on common changes and trigger packaged smoke tests or larger platform matrices on an appropriate schedule or integration boundary. Publish failed stage, affected target, and the reproducing invocation.

**Check** a known compile failure and a package-only asset failure. Confirm each reports at the intended stage, with actionable logs. Measure submission-to-result and queue time.

### Recover safely from worker failure

Isolate jobs from mutable worker state where practical. Use bounded retries for transient infrastructure errors and retain the first failure details. Publish artifacts only after required validation succeeds.

**Check** by ending a worker during build and during artifact upload. Confirm failed or partial files stay out of the normal artifact listing, then retry from the same inputs and launch the resulting build.
