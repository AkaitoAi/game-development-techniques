# Mechanic introduction sequencing

## Problem and design reasoning

A player can complete an introductory obstacle through experimentation without learning the rule needed later. The sequencing problem is deciding which prerequisite decisions the player must understand before two mechanics interact. In Nintendo's account of *Super Mario Bros.*, developers moved easier features earlier after beginners struggled with later challenges. They also designed the first mushroom's route to offer another collection opportunity. Those are documented reasons to revise an introduction around observed player behavior. [Nintendo developer interview](https://www.nintendo.com/en-gb/Iwata-Asks/Iwata-Asks-New-Super-Mario-Bros-Wii/Volume-2/4-Adjusting-the-Map-in-a-Daily-Cycle/4-Adjusting-the-Map-in-a-Daily-Cycle-233027.html)

Valve's GDC 2008 session *A PORTAL Post-Mortem: Integrating Writing and Design* provides a shipped-game developer context for coordinating level instruction and narrative. Its listing establishes the session, while the curriculum and validation workflow below is a portable recommendation. [GDC 2008 session archive](https://gdcvault.com/free/gdc-08/?media=a)

## Techniques and practical checks

### Map prerequisites to observable decisions

For each mechanic, name the decision it enables and the earlier knowledge it assumes. Represent dependencies as a small graph. Order introductions so required knowledge is available on every legal route.

**Check** trace the fastest route, an optional detour, and checkpoint reload. At the first combined challenge, list which prerequisite decisions have actually been exercised on each route.

### Establish independent evidence of learning

Introduce one unfamiliar relation at a time and provide a recoverable attempt. Follow success with a changed arrangement that requires the same rule, so memorizing a single setup cannot explain completion.

**Check** ask a first-time player to predict the second arrangement's outcome before acting. Record correct predictions, independent execution, and errors separately.

### Combine mechanics with a controlled change

Reuse familiar movement and presentation while changing the decision under test. When combining two mechanics, expose their connection before adding timing pressure or another rule.

**Check** compare failures in the individual and combined tasks. Determine whether the new failure concerns recognition, ordering, or execution before changing difficulty.

### Preserve the learning sequence after interruption

Store meaningful tutorial progress and decide what a checkpoint assumes. Restore any required resource or instruction context and retire prompts for completed steps.

**Check** leave during a prompt, fail after consuming the practice resource, and return from an optional branch. Verify the next challenge remains solvable and the player receives the relevant context.

See [Onboarding and tutorials](onboarding-and-tutorials.md) for help escalation and [Level design](level-design.md) for spatial teaching.
