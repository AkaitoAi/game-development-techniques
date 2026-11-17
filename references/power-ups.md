# Power-ups and temporary forms

## Problem and design reasoning

A pickup can change which routes and actions are possible. If its rules are unclear or the player can lose it at a critical gate, a reward can cause confusion or block progress. In *Super Mario Bros. Wonder*, Bubble form creates temporary platforms and Drill form changes how the player traverses terrain. Nintendo's GDC talk discusses creating new experiences within a familiar side-scrolling format. The design inference is to give each power-up a recognizable use and create situations where that use matters. [Nintendo gameplay presentation](https://www.nintendo.com/us/whatsnew/experience-the-unexpected-in-super-mario-bros-wonder-gameplay-presentation/) · [Nintendo, 2D and Tomorrow, GDC](https://gdcvault.com/play/1034668/2D-and-Tomorrow-How-the)

## Techniques and practical checks

- Specify whether the pickup grants a charge, timed effect, or form. Define what ends it and what a second pickup does. **Check** expiry, replacement, damage, and checkpoint reload while active.
- Place a safe opportunity to try the power before requiring precise use. **Check** whether a first-time player can describe what the pickup enables after one encounter and apply it in a changed layout.
- Treat form changes as movement changes when sizes or collision rules differ. **Check** equipping and losing a form near a low ceiling, narrow opening, moving surface, and checkpoint. Verify a legal recovery position.
- Protect completion when a power is optional or can be lost. **Check** every required route without the power, or verify that the level provides a repeatable pickup or restart path when it is required. Check simultaneous pickups in supported multiplayer modes.
