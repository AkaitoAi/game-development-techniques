# Combat design for games

## Problem and design reasoning

**Problem.** Players experience a failed fictional encounter as input friction, or several combat options resolve to the same optimal choice regardless of state.

**Design reasoning.** Motion Twin documents assistance features in Dead Cells. A design inference from those features and Street Fighter 6's shared Drive Gauge is to lower accidental command friction while preserving choices about when and where a player commits limited resources.

## Shipped examples

**Super Smash Bros. for Wii U (2014).** Nintendo's manual explains that accumulating damage increases how far a fighter is launched when hit. Scoring depends on launching opponents beyond the battlefield while they are unable to get back. Position, recovery options, and damage all inform the player's next decision. [Nintendo electronic manual, chapter 6](https://www.nintendo.com/eu/media/downloads/games_8/emanuals/wii_u_6/super_smash_bros__for_wii_u/ElectronicManual_WiiU_SuperSmashBrosForWiiU_EN.pdf)

**Dead Cells (2018).** Sébastien Bénard's postmortem lists free turnaround and auto-aiming as combat control choices. Motion Twin treated interpretation of player intent as a design tool, especially in situations where input friction would make a failed run feel undeserved. [Sébastien Bénard, GDC 2019 slides](https://media.gdcvault.com/gdc2019/presentations/Benard-Sebastian-DeepCells.pdf)

**Street Fighter 6 (2023).** Capcom launched multiple control schemes that give players different ways to access the same game. The Drive Gauge funds several distinct actions, making resource availability part of the decision to defend or apply pressure. [Capcom battle system and control overview](https://news.capcomusa.com/2022/06/02/street-fighter-6-redefines-the-genre-in-2023/)

For broader design context, Masahiro Sakurai addresses the mechanisms behind enjoyable play in his CEDEC keynote and develops risk and reward in a later creator lecture. The application here is to make each fictional combat option's payoff depend on observable state and a meaningful player commitment. [Sakurai, CEDEC 2012 keynote program](https://cedec.cesa.or.jp/2012/program/KN/C12_I0279.html) · [Sakurai, Risk and Reward](https://www.youtube.com/watch?v=FXqEykD5Ub4)

## Techniques and practical checks

### Author moves as readable commitments

Give each move an anticipation phase, an eligible contact interval, and a recovery phase. Define the simulation ticks at which input, movement, interruption, or transition is allowed. Use pose, timing, and sound to communicate significant state changes. Choose a dominant cue for effects that often overlap.

**Check** an input or contact immediately before, on, and after each boundary at supported simulation and render rates. Review the gameplay cue from normal camera distance and confirm it agrees with state even after animation blending.

### Resolve contacts through consistent rules

Define the game-space volumes that can make and receive contact, the team and invulnerability rules, and whether a move can register repeatedly against the same actor. Establish a stable tie rule for simultaneous contacts. Drive feedback from the resolved result for each participant.

**Check** a miss, blocked contact, valid hit, trade, range-boundary hit, and repeated overlap. Inspect a per-tick trace of candidate contacts, accepted outcomes, and cues. Repeat with low frame rates and reduced effects.

### Preserve meaningful choices under pressure

Give abilities distinct costs and useful situations. Alter range, commitment, repositioning, resource use, or reward so a move's value depends on the encounter. Set explicit rules for depleted resources and how players regain options.

**Check** with scripted fixtures covering close and far starts, full and depleted meters, and open space versus a boundary. Compare successful responses, recovery time, and resource spent. Investigate any single move that reliably resolves every fixture.

### Keep input assistance predictable

Choose where to buffer a command or adjust facing so assistance reflects the player's likely intent. Make target eligibility explicit, resolve ambiguity consistently, and define when a direct directional command takes precedence. Separate target choice, command acceptance, and the actual contact test.

**Check** with two equally eligible targets, a target switching sides during anticipation, and commands near the expiry of a buffer. Compare with assistance disabled where supported and confirm that both configurations preserve clear contact rules.
