# Game accessibility

## Problem and design reasoning

**Problem.** A required cue has only one sensory channel, or the control scheme demands several simultaneous inputs that some players cannot perform.

**Design reasoning.** Turn 10 documented collaboration with blind players and consultants. Blind Driving Assists and One Touch Driving show how separate options can address cue access and input demand at different points in the same gameplay loop.

Start with a task the player needs to complete, such as configuring controls, locating an objective, or reacting to a warning. Identify what the default path requires the player to perceive, understand, or physically perform. Test an alternative path through the entire task, including setup and recovery.

## Shipped examples

**Half-Life 2 (2004).** Valve supports a captions mode for audio including non-dialogue sounds and a subtitles-only mode for speech. The distinction lets players choose whether the text channel also communicates environmental events. [Valve support on subtitles and closed captions](https://help.steampowered.com/en/faqs/view/73F2-8EEA-1113-69B2)

**Forza Motorsport (2023).** Turn 10 collaborated with blind players and accessibility consultants to develop Blind Driving Assists. These supply supplemental audio cues for navigating tracks, with explanations and previews in the settings menu. One Touch Driving lets players combine specific driving assists to reduce simultaneous input demands. [Turn 10 feature breakdown](https://news.xbox.com/en-us/2023/04/27/forza-motorsport-accessibility-features-blind-driving/) · [GAConf 2023 developer session](https://www.gaconf.com/gaconf-usa-2023/)

The cases illustrate two levels of intervention. Captioning communicates the meaning of existing audio through text. Track cues expose game state that would otherwise be conveyed visually, and driving assists change the amount of simultaneous input required. Choose the intervention that addresses the observed barrier.

## Techniques and practical checks

### Provide equivalent cues for required decisions

List required information by task, such as warning direction, speaker identity, or objective range. Route a gameplay event into appropriate visual and audio cues and reserve haptics as an additional channel where supported. For captions, give speakers or important non-speech sounds distinct labels and let users control size and background. Apply readable priority and retain cues long enough to act on them.

**Check** a representative encounter with sound muted, with color distinctions suppressed, and under noisy or visually crowded conditions. Ask players to identify the source, meaning, and needed response. Verify that subtitle layout fits the largest supported text setting and a long translation.

### Customize input at the action layer

Bind devices to semantic actions and have menu prompts and tutorials read from the active binding map. Where a sequence demands sustained input or several simultaneous presses, evaluate toggle, tap, or independent assist options. Give remapping a clear conflict resolution and reset path.

**Check** using a remapped keyboard, gamepad, and any supported accessible controller. Complete onboarding, pause, recovery from an input disconnect, and a full gameplay loop with alternative bindings. Check that tutorial prompts and menu actions follow the configuration.

### Make settings usable before play

Expose essential settings at first launch and in the pause menu. Let the player preview magnification, captions, audio signals, and motion effects from the settings screen. Support usable default focus, a defined back action, and spoken names and values where menu narration is supported.

**Check** from clean profile creation with the intended assistive setup. Adjust settings, restart, and verify persistence across profile and device changes. Include a menu-only path that reaches the option without completing gameplay first.

### Test with players who encounter the barrier

Recruit players with relevant access needs and let them use their usual equipment. Observe where they need help and record whether they can discover the feature, use it under pressure, and recover from an error. Retest the same route after feature or UI changes.

**Check** using a task sheet with success criteria, such as completing a lap using track cues and independently resolving one missed turn. Capture remaining blockers and reproduce them with the same settings.
