# Behavior tree programming

## Problem and design reasoning

Adding behaviors can create conflicting actions, repeated condition work, and hidden state that survives interruption. In shipped *Halo 2*, Bungie organized behavior using a hierarchy with reusable branches. Damian Isla's GDC 2005 proceeding describes injecting short-lived stimulus behaviors into that hierarchy so normal priorities still arbitrate event responses. The aim was scalable behavior that players could understand. [Damian Isla, Handling Complexity in the Halo 2 AI, GDC 2005](https://www.gamedeveloper.com/programming/gdc-2005-proceeding-handling-complexity-in-the-i-halo-2-i-ai)

The conference session records the shipped-system context. The lifecycle protocol below is a portable implementation recommendation. It does not imply that Bungie used a generic sequence-and-selector library. [Bungie, Managing Complexity in the Halo 2 AI System, GDC](https://gdcvault.com/play/1020270/Managing-Complexity-in-the-Halo)

## Programming techniques and practical checks

### Make execution state explicit

Define whether a node starts, continues running, succeeds, fails, or aborts. Keep reusable node definitions separate from actor-owned execution memory. Specify whether a sequence restarts from its first child or resumes the running child after reevaluation.

**Check** run two actors through one shared tree asset with different targets. Verify independent node state, then force each terminal outcome and record the exact start and stop counts.

### Treat abort as a resource release

Give each running action ownership of path requests, interaction reservations, and temporary movement overrides. On abort, release these through an idempotent cleanup path. Associate asynchronous completions with the action instance so a late result cannot revive a stopped action.

**Check** interrupt movement with a higher-priority action, then complete the old path request. Verify that the actor follows the current intent and the old reservation is released once.

### Separate stimuli from immediate control changes

Convert an event into a timestamped fact or eligible response, then allow the normal priority rules to choose the resulting action. Bound the lifetime of transient stimuli and define whether repeated events replace, merge, or queue.

**Check** deliver the same stimulus during idle, combat, and a committed traversal. Verify that priorities produce the intended response and an expired stimulus cannot trigger later.

### Trace reevaluation and action ownership

Record the active branch, condition changes, abort reasons, and time spent evaluating nodes. Give expensive conditions explicit refresh rules. Add stability thresholds where competing branches would switch repeatedly near a boundary.

**Check** replay a target moving repeatedly across a threshold. Inspect switch frequency, evaluation cost, and action cleanup, then verify the trace identifies why the chosen branch changed.

See [Gameplay state machines](gameplay-state-machines.md) for transition contracts and [Asynchronous path requests](asynchronous-path-requests.md) for request ownership.
