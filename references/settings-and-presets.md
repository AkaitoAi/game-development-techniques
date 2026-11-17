# Settings and presets

## Problem and design reasoning

A long settings list makes startup difficult for people who already know the outcome they need but do not know individual setting names. A preset can provide a usable starting point. Inferred design trade-off. Presets save time, while preserving individual tuning and showing what will change lets players keep a setup that already works.

## Shipped examples

**God of War Ragnarök (2022).** The game offers high-contrast palettes that apply to multiple categories together, with controls for setting those categories separately. Santa Monica Studio describes its broader accessibility work as informed by accessibility consultants and player communities. The preset's role as a starting point for tuning is inferred from the combination of group and category controls. [PlayStation settings guide](https://www.playstation.com/en-us/games/god-of-war-ragnarok/accessibility/) · [Santa Monica Studio feature overview](https://sms.playstation.com/stories/god-of-war-ragnarok-accessibility-features)

**Forza Motorsport (2023).** Turn 10 describes the Global Assist Preset as a way to adjust grouped settings easily. Its Blind Driving Assists provide an initial set and allow individual tuning. Turn 10's GAConf talk describes developing the assists with members of the gaming and disability community. [Turn 10 settings guide](https://support.forza.net/hc/en-us/articles/20964254277267-Forza-Motorsport-Accessibility-Support) · [GAConf USA 2023 program](https://www.gaconf.com/gaconf-usa-2023/#:~:text=Accessibility%20Journey%20for%20Forza%20Motorsport)

## Techniques and practical checks

### Make presets predictable

Implement each preset as a named set of values with a declared scope, such as accessibility, controls, or graphics. Preview the affected settings before applying it when changes are disruptive. Mark the result as custom after a user edit, and keep defaults for comparison.

**Check** start with a bespoke setup, apply a preset and change one value. Verify the custom state and confirm that controls outside the preset's scope retain their values. Apply a second preset and compare the changed values with its preview.

### Make risky changes reversible

Keep the most recent usable display settings until the player confirms a mode change. On timeout, restore that configuration. Provide a recovery path if custom bindings or input device changes make a menu unusable.

**Check** switch to an unsupported display mode, unplug the active controller, and wait out the confirmation prompt. Verify that a usable view and input path return without confirmation.

### Keep dependencies visible

Model dependencies and capability limits in setting definitions. Explain why a child setting is inactive and show the effective value when another setting overrides it. Keep inactive preferences available for restoration when a supported configuration returns.

**Check** set a dependent value, disable its parent option, and re-enable it. Repeat on a build without the required capability. Check whether the displayed reason and actual runtime behavior match.

### Version saved preferences and test real use

Store a settings schema version and separate device-specific values from account preferences. Preserve existing customizations when updating defaults or migrating setting names. Provide reset per category and run a task in gameplay after making a change.

**Check** launch with an older settings file, change inputs and display, restart, and reset only one category. Check effective values in play, including subtitles, control mapping, and a setting that requires restart.
