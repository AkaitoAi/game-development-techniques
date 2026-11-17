# Mobile offline play and unreliable networking

## Problem and design reasoning

Players switch between Wi-Fi and cellular, enter coverage gaps, suspend the game, or run out of time waiting for a response. A network indicator and a server error have different meanings, and an operation may have succeeded even when its response never reached the device. In their Tunnel Town GDC Next session, Smart Bomb Interactive developers describe the risk that an ambiguous choice between save states can cost hours of progress. Supercell documents that competitive games instead need explicit behavior when a player loses connection mid-match. The design inference is to classify features by authority, then let local features continue, show pending changes honestly, and recover server-owned outcomes from verified state. [Smart Bomb Interactive, “Syncing Success,” GDC Next 2013](https://www.gdcvault.com/play/1019692/Syncing-Success-Managing-Online-Offline) · [Supercell connection behavior](https://support.supercell.com/clash-of-clans/en/articles/unstable-connection.html)

## Shipped examples

- **Tunnel Town.** Developers Dave Geurts and Colter Haycock drew on this mobile game for their GDC Next talk about offline play and cross-device state. The session identifies mistaken save selection as a design problem and describes the team pursuing a simpler player experience. Its overview does not specify the title's synchronization algorithm. [GDC Next 2013 session](https://www.gdcvault.com/play/1019692/Syncing-Success-Managing-Online-Offline)
- **Clash Royale and Brawl Stars.** Supercell documents two different disconnection policies. A disconnected Clash Royale player cannot deploy cards while the match continues. Brawl Stars uses a bot and can return control if the connection comes back before the match ends. Graceful fallback depends on the mode's rules and what the game can safely delegate. [Supercell multiplayer support](https://support.supercell.com/clash-of-clans/en/articles/unstable-connection.html)

## Techniques and practical checks

### Declare what works offline

Separate device-owned play, locally queued actions, and server-owned actions. Let the client show cached content with an age or freshness state where it matters. Show which modes require a connection before entering them. Matchmaking, purchases, and shared rewards should report pending, confirmed, or unavailable status based on what the server has acknowledged.

**Check** an airplane-mode cold start and a coverage loss inside each major mode. Verify that playable content remains reachable, that blocked actions explain what is unavailable, and that the interface never presents a pending reward as final.

### Make retries safe

For durable queued actions, persist an operation ID and the data needed to resume. Have the authoritative service handle repeated submissions of the same ID as one operation and report the recorded result. Use bounded backoff with jitter for retryable failures. Refresh authentication or ask the player to resolve permanent failures through the appropriate UI path.

**Check** a controlled request whose server commit succeeds while its response is lost. Kill and relaunch the game before retrying. Confirm one server-side outcome and a clear final status on the device. Repeat with an expired session and an invalid action.

### Reconcile on a connection change

Use connectivity notifications as hints and prove the service path with a request. After reconnect, obtain an authoritative snapshot or updates after the last acknowledged revision, depending on the protocol. Set timeouts and define the match's fallback, return-to-lobby behavior, and reconnection deadline.

**Check** switch Wi-Fi to cellular and back in a test match, then repeat with packet delay, a limited outage, and an outage past the deadline. Verify the game enters and leaves the reconnect state and restores only valid inputs, events, and outcomes.

### Budget mobile network work

Keep the critical path to play small and schedule optional downloads or bulk synchronization after entry when possible. Bound cache age and queue growth. Respect player or device preferences for costly transfers. Set request deadlines and budgets that leave room for gameplay traffic.

**Check** first launch and repeat launch on throttled cellular, then background during an optional transfer. Measure time to interactive, bytes transferred, retry frequency, and queue size after reconnection. Verify that the game can reach a useful screen under the expected connection limits.
