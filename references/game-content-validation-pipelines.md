# Game content validation pipelines

## Problem and design reasoning

An asset can import successfully on its own and still break a level, exceed a target platform budget, or leave a packaged build with missing dependencies. Check the authored files early for local mistakes, then check the combined content after overrides, imports, and platform selection. This gives authors fast feedback and lets the release gate see what players will actually receive.

Wube documents that *Factorio* applies successive mod prototype stages before the engine reads and validates the resulting data. Its lifecycle also records which mods changed a prototype. The transferable inference is to validate the merged result and retain the provenance needed to repair it. [Factorio Data](https://lua-api.factorio.com/latest/types/Data.html) · [Factorio Data Lifecycle](https://lua-api.factorio.com/latest/auxiliary/data-lifecycle.html)

At GDC, Black Shamrock technical director Eoin O'Grady described using asset health metrics on *Voyagers of Nera* to make suboptimal configurations visible and target optimizations before Early Access. That case supports using reports to direct remediation, with a release gate reserved for defects or budgets the team has agreed are blocking. [Smooth Sailing: Using Asset Health Metrics to Optimize Voyagers of Nera](https://www.gdcvault.com/play/1036065/Smooth-Sailing-Using-Asset-Health)

## Techniques and practical checks

### Validate at the stage with the necessary evidence

Check a file's schema or import settings on save or import. Validate resolved references and platform-specific constraints during the cook. Run packaged smoke tests for problems visible only in the delivered artifact. Reuse rule implementations where the relevant inputs and semantics match.

**Check** a malformed source asset, an override that invalidates a reference, and a dependency missing from the package. Confirm the appropriate stage rejects each one with a path to reproduce it.

### Keep incremental results tied to their inputs

Key validation results by the content and relevant dependency revisions, rule version, platform, and importer settings. When a shared dependency changes, recheck its known dependents. Keep a clean validation path for release candidates.

**Check** change a shared skeleton or material while leaving the referencing assets untouched. Confirm dependent files are revalidated and incremental findings match a clean run.

### Report faults in authoring terms

Report the asset, effective setting, responsible source or override, rule ID, and the expected value. Classify findings by impact and give exceptions a reason and scope. Use measurable thresholds on representative targets when checking budgets.

**Check** create a texture whose source settings pass but whose platform override exceeds a budget. Confirm that the report identifies the responsible override and that a narrowly scoped exception does not mute unrelated assets.

### Verify fixes and release decisions

Offer previews for bulk changes and use version control to review them. Compare reports across candidate builds by stable rule and asset identifiers. Assign blocking severity to known shipping failures and monitor new warnings by change.

**Check** run a dry-run fix over a mix of compliant and failing assets, apply the approved changes, and run validation twice. Confirm the second run proposes no further edit and the packaged build loads the affected content.
