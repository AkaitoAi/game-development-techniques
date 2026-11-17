# Gameplay invariants and validation

## Problem and design reasoning

A game can produce a state that its systems assume is impossible. If the first visible symptom appears several commands later, a replay or crash dump may obscure the original error. Wube describes a *Factorio* case where overlapping deconstruction and blueprint plans exposed assumptions about what could coexist in a tile. The developer tightened the rule and added consistency checks to learn about violations near their origin, before investigating more desync reports. [Wube Software, Friday Facts #242](https://www.factorio.com/blog/post/fff-242)

Santa Monica Studio's TestMonkey presentation describes running gameplay and determinism suites in continuous integration. Use this as evidence for the testing workflow. The split below between checks in live game code, content validation, and regression exercises is an implementation recommendation. [Ben Hines, TestMonkey: Automated Testing at Santa Monica Studio, GDC 2023](https://www.gdcvault.com/play/1029379/Tools-Summit-TestMonkey-Automated-Testing)

## Techniques and practical checks

- **State the rule and where it must hold.** For example, every equipped item must belong to its character's inventory at the end of an equipment command. Define the command boundary so a check allows valid intermediate steps within a transaction. **Check** a successful transfer, a full destination, and two queued transfers of the same item. Verify that the source and destination satisfy the rule after each accepted or rejected command.
- **Reject invalid input at entry.** Validate authored references, uniqueness, and ranges during import or build. Validate runtime requests before changing game state. **Check** a missing item definition, duplicate identifier, and a command using an expired item reference. Verify that diagnostics identify the affected data or command and that existing progress stays readable.
- **Report the first violation close to the write.** In diagnostics builds, attach the responsible command, tick, and relevant entity IDs to a failed check. Use local checks at frequently used boundaries and reserve expensive full-state scans for test runs. **Check** introduce a controlled invalid transfer in a test fixture. Verify that the diagnostic identifies the command that created it and that a separate validation pass finds the same inconsistency.
- **Exercise combinations and valid edge cases.** Build sequences from legal actions and check the invariant after each transaction. Keep the seed and shortest reproduced action sequence for a failure. **Check** generate combinations of item transfer, equipment change, save and reload, and object removal. Confirm that expected failed actions preserve state and that valid boundary states pass.
