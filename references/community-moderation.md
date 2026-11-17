# Community moderation and player safety

## Problem and design reasoning

Players need ways to stop unwanted contact immediately, while reviewers need enough context to apply rules consistently. Warning signs include players abandoning reports, moderators receiving reports without usable context, and penalties that provide no clear explanation. Riot documented separate goals in its early *League of Legends* work, including reforming harmful behavior and reinforcing positive conduct. For *VALORANT*, Riot described evidence review as a way to substantiate voice reports, with different handling for data no longer needed for review. A transferable inference is to give immediate player controls, reporting, investigation, and appeals separate workflows with their own success measures.

## Shipped examples

- **League of Legends.** In a GDC presentation, Riot's Jeffrey Lin discusses the historical player-driven Tribunal and Honor Initiative as interventions intended to address harmful behavior and encourage positive conduct. These are examples from that period of the game. [Riot developer talk, GDC Vault](https://www.gdcvault.com/play/1017940/The-Science-Behind-ShapingPlayer)
- **VALORANT.** Riot described the rationale for evaluating relevant voice evidence and notifying players when reports lead to action. Riot's support documentation describes communications restrictions and evidence retention. [Riot's 2021 rationale](https://www.riotgames.com/en/news/updating-the-privacy-notice-and-terms-of-service) · [VALORANT support](https://support.riotgames.com/en-us/valorant/penalties/comms-restrictions-and-audio-retention/)
- **Roblox.** Roblox documents in-experience reports for a person or an experience, in addition to reporting chat and items elsewhere in the platform. This exposes reporting where the player encounters the behavior or content. [Roblox reporting and blocking](https://about.roblox.com/reporting-and-blocking)

## Techniques

### Give players immediate controls

Make mute and block reachable from a match roster and recent-player list. Specify how each affects current communication, future invitations, and cross-device preferences. Provide a separate report action and allow the player to leave the interaction while the case is pending.

**Check** with gamepad, keyboard, and assisted navigation. Mute a player during a match, change devices, and verify the expected scope survives reconnection. Confirm that the disruptive player receives no private information about the reporter.

### Capture bounded context for review

Attach the game or content identifier, relevant event time, chosen policy category, and applicable excerpt. Set access roles and retention by evidence type and purpose. Restrict exports and log evidence access. Show players a useful report receipt without exposing private reviewer notes.

**Check** report the same synthetic incident through two entry points and confirm that context reaches the correct case. Run expiry and access tests using mock data and verify interfaces preserve the reporter's confidentiality.

### Define graduated actions and appeals

Document when to use a warning, feature restriction, or broader account action. Give reviewers a clear severity rubric, explain the applicable rule to the affected player, and route appeals to a reviewer who can overturn the action. Require contextual review when an automated signal is ambiguous or an action has serious consequences.

**Check** run anonymized fixtures covering ambiguous language, repeated misconduct, and false reports through independent reviewers. Measure agreement, verify restrictions end as specified, and exercise restoring access after a successful appeal.

### Measure more than report volume

Track time to triage and resolution, appeal overturns, repeat offenses, and player-reported safety alongside report counts. Segment by platform, language, and reporting route where there is enough data to protect privacy. A reporting redesign may raise counts simply by making the action easier to find.

**Check** a staged reporting UI change using exposure-normalized metrics and moderated usability tests. Review sampled outcomes before interpreting a rise or fall in report volume.
