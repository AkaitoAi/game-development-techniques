# Game design patterns for information and sessions

## Problem and design reasoning

Players need enough information to make a decision and a clear way to stop and resume a session. Exposing every possible detail competes with the immediate action. Hiding the rules can make outcomes feel arbitrary. In *Into the Breach*, showing impending attacks gives players a concrete board problem to solve. Matthew Davis describes the iteration and balance work behind the shipped game. [Subset Games design postmortem, GDC](https://www.gdcvault.com/play/1025772/)

Housemarque documented adding a single-use suspend point to *Returnal* so players could leave a cycle and resume later while preserving its structure. A transferable design inference is to choose session boundaries based on what a player should be able to pause, revisit, or commit to. [Housemarque, Returnal 2.0](https://blog.playstation.com/2021/10/26/returnal-2-0-update-brings-suspend-cycle-and-photo-mode/)

## Pattern catalog

| Pattern | Problem it addresses | Approach and trade-off |
|---|---|---|
| Telegraph | A threat arrives before players can respond | Signal direction, timing, or category ahead of impact. More warning makes reactions fairer and can reduce surprise. |
| Outcome preview | A choice is difficult to evaluate | Show the predicted state change before committing. Previews need reliable access to resolution rules. |
| Partial information | Full knowledge makes decisions routine | Withhold selected facts while exposing how to learn them. Unbounded uncertainty discourages planning. |
| Shared information | Teammates lack a common view | Reveal team-critical state to all teammates. A compact display helps prevent competing signals. |
| Private information | Disclosure would remove strategic choice | Limit what each role can see and define when information is revealed. Spectator and replay policies must honor that rule. |
| Progressive disclosure | The interface presents too much at once | Show essential status by default and offer details on request. Important actions must stay discoverable. |
| Redundant cues | A single sensory channel can be missed | Encode the same fact in two or more available channels. Keep cues synchronized so they communicate a consistent state. |
| History log | Players cannot tell why a result changed | Record important actions and outcomes in order. Avoid burying decision-relevant entries in trivial events. |
| Confirmation before commitment | A misinput has an expensive consequence | Confirm costly, unusual, irreversible choices. Frequent confirmations can create habitual dismissal. |
| Undo window | A reversible mistake consumes a whole session | Allow reversal before revealing new information or affecting other players. Define what closes the window. |
| Checkpoint | A failure repeats too much solved material | Restore at a meaningful retry boundary. A checkpoint too close to the action may omit needed setup or recovery space. |
| Suspend and resume | Players need to leave during a long run | Preserve current state and explicitly define when it can be resumed. A single-use policy adds recovery cases after a crash. |
| Quick session | Players have limited or unpredictable time | Offer an activity with a predictable stopping point. It needs a complete player decision or payoff within that time. |
| Session recap | Returning players have lost their context | Summarize the current goal and recent changes. Tailor the recap to the actual state rather than replaying generic instructions. |
| Debrief | Players need feedback after a match or mission | Show a few meaningful outcomes and connect them to actions. Careless metrics can reward behavior that undermines the objective. |

## Practical checks

- **Check** ask first-time players to predict the next outcome in three scenarios. Compare their explanations with the preview or telegraph and the actual result.
- **Check** play the same encounter with a cue channel disabled. Record which important events players can still identify, including simultaneous events.
- **Check** interrupt immediately before and after a commit, a checkpoint, and a suspend. Resume each case and verify the expected state and reward ownership.
- **Check** bring a player back after a break. Observe whether the recap helps them name the next goal and resume play without coaching.
