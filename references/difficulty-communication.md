# Difficulty communication

## Problem and design reasoning

A player can fail because an intended challenge is demanding, or because the required response is hard to perceive. Those causes need different interventions. For shipped *God of War Ragnarök*, Adam Oliver describes preserving expressive combat while resolving unintentional barriers. Santa Monica's feature overview documents audio cues for unblockable attacks and adjustable presentation. The practical inference is to test whether players recognize the required action before changing the action's difficulty. [Adam Oliver, Breaking Barriers: Combat Accessibility in God of War Ragnarök, GDC 2023](https://www.gdcvault.com/play/1028726/Breaking-Barriers-Combat-Accessibility-in) · [Santa Monica Studio, accessibility features](https://blog.playstation.com/?p=364812)

This guide concerns how a challenge communicates its demand and outcome. For changing the underlying demand, see [Difficulty and balancing](difficulty-and-balancing.md). For sensory presentation, see [Color and visual readability](color-and-visual-readability.md).

## Techniques and practical checks

### Describe the action the player must understand

For each challenge, write down the stimulus, legal responses, commitment deadline, and consequence. Give the cue an explicit connection to that rule. If an encounter distinguishes blocking, dodging, and interrupting, choose recognizable cues that remain meaningful when several threats overlap.

**Check** show a first-time player a representative action and pause before its commitment deadline. Ask what response they expect to work, then let the action resolve. Compare recognition failures with execution failures and change the relevant part.

### Communicate what a setting changes

Describe concrete effects such as incoming damage, timing allowance, or navigation guidance in the settings UI. Explain when the change applies. Let a player inspect the current effective setting after a preset and an individual override interact.

**Check** ask players to predict the result of changing one option, then demonstrate it in the same fixture. Repeat after applying a preset and changing an individual option. Verify the effective rule matches the description and survives the promised save scope.

### Make failure informative

Show the consequence promptly and retain enough context for a retry. Use a brief cue or optional explanation when two superficially similar failures follow different rules. Keep the explanation specific to the observed cause and allow players to dismiss repeated help.

**Check** create fixtures for a correct response made too late, an ineligible response, and a missed input. Review traces and video together. Confirm feedback identifies the relevant failure without contradicting the actual resolution.

### Test communication under real encounter load

Evaluate the cue at gameplay scale, camera distance, and effect density. Give critical information a supported alternative channel. Define how offscreen or simultaneous threats communicate their urgency so the UI does not imply a response window that has already ended.

**Check** replay the same threat alone, during a crowded encounter, and with one presentation channel unavailable. Record recognition rate and time to first correct response. Report the test setup, participant observations, and execution traces separately.
