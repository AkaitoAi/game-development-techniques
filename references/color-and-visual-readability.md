# Color and visual readability

## Problem and design reasoning

Players need to find actionable information under motion, changing light, effects, and different display conditions. Color can reinforce a distinction, but two colors can appear similar in some conditions and a costume or background can erase the intended contrast. Decide which categories must be recognizable at a glance, then build visual separation into silhouettes, local luminance, shape, and interface symbols.

**Documented reasoning.** Valve explicitly designed distinct class silhouettes and shading to support quick character recognition in different lighting conditions. Naughty Dog describes its high-contrast render mode and HUD adjustments as responses to elements being difficult to see or read. [Valve, Illustrative Rendering in Team Fortress 2, NPAR 2007](https://cdn.fastly.steamstatic.com/apps/valve/2007/NPAR07_IllustrativeRenderingInTeamFortress2.pdf) · [Naughty Dog launch feature guide](https://blog.playstation.com/2020/06/09/the-last-of-us-part-ii-accessibility-features-detailed/)

**Transferable inference.** Test category recognition as a gameplay outcome. A pleasing palette in a style guide does not establish that players can distinguish the relevant characters, objects, or UI states during a scene.

## Shipped examples

- **Team Fortress 2 (2007).** Valve validated class designs with silhouettes before relying on interior details, and used rim highlights, hue, and luminance variation to communicate shape. Its world art also uses restrained detail to reduce visual noise. [Valve, Illustrative Rendering in Team Fortress 2, NPAR 2007](https://cdn.fastly.steamstatic.com/apps/valve/2007/NPAR07_IllustrativeRenderingInTeamFortress2.pdf)
- **The Last of Us Part II (2020).** An optional High Contrast Display mutes environmental color and emphasizes allies, enemies, items, and interactable objects. The game also exposes HUD color, background, size, and colorblind palette controls. [Naughty Dog launch feature guide](https://blog.playstation.com/2020/06/09/the-last-of-us-part-ii-accessibility-features-detailed/)

## Techniques and practical checks

1. **Define what each cue must communicate.** Give meaningful states a stable category and combine hue with shape, pattern, icon, outline, or text. Use a consistent state cue across the world and HUD where both represent the same information. **Check** screenshots in grayscale and representative color-vision simulations, then ask players with relevant color-vision differences to identify the actual states during play. Record confusion pairs.
2. **Validate silhouettes and their smallest useful scale.** Strip internal detail and compare roles from gameplay viewpoints, including common animation poses and partial occlusion. Restore lighting and effects to confirm the outline still separates from the background. **Check** short encounter clips at expected screen sizes, distances, and graphics presets. Record which roles viewers identify incorrectly before they see labels.
3. **Control contrast in the final image.** Reserve higher local contrast for immediate gameplay information and reduce competing detail behind important cues. Evaluate visible contrast after exposure, tonemapping, bloom, and motion effects. **Check** the same interactable or hazard against the brightest and darkest expected backgrounds, mid-transition as well as at rest. Repeat in supported SDR and HDR modes where applicable.
4. **Design an alternate high-contrast presentation.** Classify accessible highlights by semantic role, and define priority if an entity qualifies for several cues. Keep objective markers and selected UI elements readable in that mode. **Check** toggle behavior during a moving encounter, after respawn, and after restarting the game. Test occluded objects to confirm the mode respects the intended game rules and use timing or error counts to evaluate whether it helps players.
