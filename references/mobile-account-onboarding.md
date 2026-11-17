# Mobile account onboarding

## Problem and design reasoning

Mobile players may start on one device and return on another. Requiring credentials too early can interrupt play, while ambiguous linking can put progress at risk. Supercell documents optional account setup and cross-device access for games such as *Brawl Stars*. Apple discusses platform game features in its WWDC session. A design inference is to request identity when the player can see the value and make the current progress owner explicit. [Supercell ID](https://supercell.com/en/supercell-id/) · [Apple WWDC25, Level up your games](https://developer.apple.com/videos/play/wwdc2025/209/)

## Techniques and practical checks

### Decide when identity is needed

Allow a durable guest profile where the game permits and explain which features require sign-in.

**Check** Complete the first session offline, kill the app, and relaunch. Check guest state and the explanation on an account-only feature.

### Make linking explicit

Display the relevant progress summaries before choosing between conflicting profiles. Apply the link as one recoverable operation.

**Check** Attempt linking when both profiles have progress. Cancel and retry, and verify the selected profile matches the confirmation.

### Handle interruption safely

Keep linking state across app switches where appropriate and resume only within the original request context.

**Check** Switch out to get a code, kill the app, then reopen. Verify expired requests can restart without losing progress.

### Test ownership changes

Give sign-out and switching a clear confirmation when unsynced progress exists.

**Check** Switch accounts on a shared device and verify that settings or cached state intended to be account-specific follow the correct profile.
