# UI number formatting

## Problem and design reasoning

Numbers influence purchases, timing, comparisons, and survival decisions. A display can mislead when rounding, units, or abbreviation hides a meaningful difference. Housemarque's Johannes Koski describes *Returnal*'s UI priorities around survival information and low-health communication. This establishes the player-decision context for the formatting recommendations below; the source does not document Returnal's numeric implementation. [Returnal UX developer account](https://blog.playstation.com/2021/05/11/unpacking-returnals-ux-design-gameplay-first-ui-retro-futuristic-tech-and-accessibility/)

Apple's WWDC20 talk *Formatters: Make data human-friendly* explains locale-aware numbers and measurements. Its relevant engineering principle is to format structured values through an explicit policy that respects locale and units. [WWDC20 developer talk](https://developer.apple.com/videos/play/wwdc2020/10160/)

## Techniques and practical checks

### Specify a display contract

For every numeric field, define its underlying unit, range, precision, rounding rule, sign, abbreviation, and invalid-value behavior. Retain authoritative values as numeric data. Keep affordability and gameplay calculations consistent with the displayed explanation.

**Check** test zero, negative values where supported, the largest supported value, and values immediately around rounding boundaries. Confirm no displayed price implies an unavailable purchase is affordable.

### Match precision to decisions

Use sufficient detail when players compare nearby values. Compact magnitudes can suit an overview, with full values available where decisions require them. Define timer rounding so the displayed remaining time communicates availability correctly.

**Check** compare values around 999/1,000 and abbreviation transitions. Test a countdown near zero and two upgrades whose rounded overview values appear equal.

### Use locale-aware formatting

Apply the selected locale's grouping, decimal separators, plural forms, and measurement conventions through a shared formatter. Include units in the contract and distinguish converted presentation values from simulation units.

**Check** exercise supported locales, right-to-left layouts, preferred text sizes, long unit labels, and maximum-width values. Verify separators and unit placement against locale fixtures.

### Update related displays coherently

Present a consistent snapshot across numeric text, bars, eligibility indicators, and accessible descriptions. Define how animated counting behaves during rapid changes, pauses, and reopening a panel.

**Check** trigger rapid gains and spending, pause, resume, change locale, and reopen the screen. Compare every representation with the authoritative value and ensure accessible output includes the useful precision.
