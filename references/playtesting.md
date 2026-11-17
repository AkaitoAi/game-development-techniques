# Playtesting and player research

## Problem and design reasoning

**Problem.** A team sees where players quit but cannot tell what they misunderstood, or internal testers complete a task because they already know the intended flow. Observe a first attempt and record what the player saw and tried before intervening.

**Design reasoning.** Valve describes iterating on Portal after watching players learn unfamiliar rules. Coin Crew describes making tests part of routine production. The practical trade-off is to run smaller studies while changes are still cheap and use a broader player sample when a decision depends on audience differences. See the shipped examples below for sources.

## Shipped examples

**Portal (2007).** Valve's creators described iterative testing in their postmortem. Watching players encounter unfamiliar mechanics gave the team evidence it could use to refine the sequence through which players learned the rules. [Kim Swift, Erik Wolpaw, and Jeep Barnett, developer postmortem](https://media.gdcvault.com/GD_Mag_Archives/GDM_January_2008.pdf)

**Escape Academy (2022).** Coin Crew Games reported running over 200 playtest sessions. The founders discussed making tests part of routine production and improving the efficiency of those sessions. [Mike Salyh and Wyatt Bushnell, GDC 2023 Independent Games Summit](https://www.gdcvault.com/play/1029191/Independent-Games-Summit-Never-Skip)

**Hades (2020).** Supergiant designed the game for Early Access. Its modular structure and serial narrative gave the team ways to incorporate community feedback while building toward version 1.0. Community feedback gives naturalistic signals from people who chose to play. A targeted session can answer questions about players who never would have joined the community. [Supergiant Games development FAQ](https://www.supergiantgames.com/blog/hades-faq/)

Steve Bromley describes planning research around development decisions and mapping playtests across a project. Scheduled sessions give teams a way to resolve important uncertainties while the related changes remain affordable. [Steve Bromley's UKIE Insight Into Games and GameGathering talks](https://gamesuserresearch.com/steve-bromleys-talks-articles-books/)

## Techniques and practical checks

### Make the research question observable

Identify the decision the team needs to make, the uncertainty that blocks it, and the behavior that would change the decision. For a new interaction, use a realistic goal and observe whether players discover the action, execute it, and understand its effect. Define a stopping condition if players reach an impasse.

**Check** by running the task with one pilot participant. Confirm the relevant behavior occurs naturally in the scenario and that the starting state presents the intended choice.

### Recruit for the behavior being studied

Screen for experience, input device, or access needs that could change the result. Keep first-time players separate from people who already know the game. For cooperative play, decide whether pairs should be strangers or established teammates based on the intended audience. Obtain informed consent for recordings, limit access to raw footage, and agree on retention.

**Check** that the roster covers the intended player groups and that recorded observations can be matched to build and cohort without putting personal information in the analysis notes.

### Observe first and probe after the attempt

Use consistent, neutral instructions. Record what the player did, what the game showed, help given, and the outcome. Ask what the participant expected once the action resolves. Separate observed behavior, participant explanations, and team interpretations in the notes.

**Check** two observers' notes for the same short clip. Resolve disagreements about the event definition and distinguish unaided attempts from attempts after coaching.

### Prioritize evidence and retest

Summarize who encountered each issue, the context, the effect on progression, and the fidelity of the evidence. Combine observations with telemetry when measuring how widespread a behavior is. Treat small qualitative samples as evidence about possible causes and behaviors. Retest proposed changes with fresh players on the same task and check for new problems introduced by the fix.

**Check** whether the original behavior improved in the follow-up and whether completion, comprehension, or enjoyment changed elsewhere. Document remaining uncertainty and the decision supported by the evidence.

### Compare two interface versions without teaching the answer

Give players a goal such as equipping an item they already own. Describe their starting state without naming the menu, button, or path. If first-time discovery is the question, assign each participant one version. If the same participants need to compare both, vary version order and use comparable tasks to reduce practice effects. Define what counts as independent completion and record any rescue prompt as an intervention. Probe afterward for what players expected and why. [Steve Bromley, moderating playtesting sessions](https://www.stevebromley.com/blog/2015/01/06/some-things-ive-learned-about-moderating-playtesting-sessions/)

**Check** a pilot recording for accidental clues in the task wording and moderator responses. Verify that independent completion, assisted completion, and abandonment can be scored from the recording. Keep the same scoring rules when rerunning the task after a fix.
