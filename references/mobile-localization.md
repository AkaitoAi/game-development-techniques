# Mobile game localization

## Problem and design reasoning

A mobile game can expose several language settings while showing content in only one effective language. Changing the phone language, applying an app override, or switching languages offline can leave screens, fonts, and event copy out of sync. The shipped Pokémon GO help guide documents an in-game language selector. That gives players a game-specific choice. Treating the selected language as shared state across UI, assets, and server content is an implementation inference. [Pokémon GO Help Center](https://niantic.helpshift.com/hc/en/6-pokemon-go/faq/2546-how-do-i-change-my-pokemon-go-app-s-language/?l=en-IE&p=web)

Android documents system-level per-app language preferences. Apple's WWDC session demonstrates testing localized interfaces with pseudolanguages. Use a consistent locale policy in the game and exercise layout on real phones before release. [Android per-app language preferences](https://developer.android.com/guide/topics/resources/app-languages?hl=en) · [Apple WWDC19, Creating Great Localized Experiences with Xcode 11](https://developer.apple.com/videos/play/wwdc2019/403/)

## Techniques and practical checks

### Resolve the effective language in one place

Define how the device preference, OS app override, in-game setting, and project fallback interact. Resolve language, script, and region explicitly when they affect assets or text. Apply the result consistently to menus, gameplay, help screens, and incoming live content.

**Check** device-language changes, a supported app override, an unsupported regional variant, and a cold restart. Verify the same effective locale across screens and confirm that progress remains accessible.

### Test expanded and bidirectional text

Use localized message templates for plurals and variable ordering. Supply fonts for the supported scripts, preserve logical reading order, and mirror directional UI where it carries navigational meaning. Give translated strings enough space to wrap or reflow.

**Check** on the smallest supported phone with large text, expanded pseudolocalized strings, and a supported right-to-left locale when available. Include a mixed-script player name, a timer, line wrapping, and a dialog with three choices.

### Make language pack changes recoverable

Bundle the core text and fonts needed to launch and navigate. If voice or high-volume art arrives as an optional pack, track locale and content version together. Keep a usable fallback until the selected pack is ready and allow the player to resume or cancel the download.

**Check** a language switch on a fresh offline install, interrupted audio download, and low-storage device. Confirm that menus stay readable, subtitles still match the selected text language, and a retry installs the right pack.

### Verify live content and the store handoff

Give translators context for short labels and time-sensitive events. Test store prices against platform-supplied localized values and localize event dates and number formatting for the actual locale and time zone. Define the fallback for missing event translations and keep it consistent through the CTA and linked screen.

**Check** a bundled tutorial, live banner, store dialog, and push deep link in the release build for representative locales. Verify dates, prices, font coverage, and fallback behavior after changing language mid-session.

For translation planning and language QA, see [Localization](localization.md). For voiced assets, see [Voice production and localization](voice-production-and-localization.md).
