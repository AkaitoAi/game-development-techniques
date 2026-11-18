# Target selection programming

## Problem and design reasoning

Aiming or interaction selection can oscillate between nearby candidates, retain a removed target, or choose something the player cannot see. The shipped *God of War Ragnarök* settings explicitly distinguish reticle-based aim assistance and lock-on behaviors, including options that automatically choose another target after a death. These are separate player-facing contracts that should be represented in selection policy. [Santa Monica Studio, Ragnarök accessibility options](https://www.playstation.com/en-my/games/god-of-war-ragnarok/accessibility/)

Adam Oliver's GDC 2023 talk describes the team's work to remove unintended combat barriers while preserving intended combat design. The candidate pipeline and hysteresis below are implementation recommendations inferred from that problem, rather than documented internal algorithms. [Santa Monica Studio, Breaking Barriers: Combat Accessibility in God of War Ragnarök, GDC 2023](https://www.gdcvault.com/play/1028726/Breaking-Barriers-Combat-Accessibility-in)

## Programming techniques and practical checks

### Filter eligibility before scoring

Gather candidates from a bounded spatial or screen-space query. Filter by action capability, team, range, and current target state before expensive visibility checks. Store target identity and a chosen aim or interaction point separately, including sub-target identity when the action supports it.

**Check** place an eligible target behind an ineligible foreground object, remove a candidate between gathering and commit, and recycle an entity slot. Verify the final action revalidates identity and eligibility.

### Express the ranking in inspectable terms

Normalize useful inputs such as reticle distance, world distance, and facing angle before combining scores. Apply explicit priority tiers for qualitatively different target categories. Log candidate rejection reasons and component scores so tuning can explain a winner.

**Check** use a fixed arrangement with one near-edge candidate and one more distant centered candidate. Sweep the reticle and aspect ratio, then confirm the chosen result matches the intended policy for each control mode.

### Stabilize automatic switching

Keep the current target while it remains valid unless a new candidate beats it by a defined margin. Specify a grace period for temporary occlusion, plus immediate release conditions such as destruction or a scene transition. Treat an explicit target-switch input as a distinct request with a directional rule.

**Check** cross two moving targets, briefly hide one behind a post, and repeatedly switch left and right. Measure switch frequency and verify that a removed target releases immediately while brief occlusion follows the chosen rule.

### Share selection state with the interface and action

Publish the selected identity, point, policy, and selection revision. Let the reticle or prompt display that revision, and revalidate it when the action commits. When camera assistance is enabled, define its response to manual camera input and expose the supported player settings.

**Check** change an assistance setting mid-encounter, turn the camera manually, and trigger an action while the selected target exits range. Verify that the indicator, camera behavior, and actual recipient agree or clearly communicate a refused action.

For contextual actions see [Contextual interaction](contextual-interaction.md). For query scheduling see [Physics query architecture](physics-query-architecture.md).
