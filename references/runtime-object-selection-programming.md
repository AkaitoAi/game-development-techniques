# Runtime object selection programming

## Problem and design reasoning

A visible object, its collision shape, and the gameplay object that owns it can be different entities. A hover query can therefore identify a decorative child while a click arrives after that child has been removed. The released Source SDK includes a crosshair helper that traces from the local player's eyes, ignores the player, requires a non-world entity hit, and returns that entity to diagnostic commands. This is concrete picking evidence from Valve's shipped engine family. It establishes the query and hit filtering, while the selection ownership protocol below is a portable recommendation. [Valve, Source SDK crosshair entity selection](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/client/c_baseentity.cpp)

Yee Cheng Chin's GDC 2015 talk describes the HTML/CSS/JavaScript UI shipped in *SimCity* (2013), including integration and tooling costs. It provides context for an interface layer communicating with gameplay objects. It does not establish the exact picking arbitration proposed here. A useful inference is to make the boundary between pointer ownership, selected object, and gameplay command explicit. [Maxis, How to Implement AAA Game UI in HTML and JavaScript, GDC 2015](https://www.gdcvault.com/play/1022054/How-to-Implement-AAA-Game)

## Programming techniques and practical checks

### Give the active interaction layer input ownership

Resolve whether a pointer belongs to a modal, a drag, a world view, or another active tool before generating a pick. Carry pointer and interaction identity through press, move, release, and cancellation. Preserve a captured drag's owner until it releases or is explicitly cancelled.

**Check** press a world object, move across a UI panel, then release. Repeat with a modal opening during the drag and a touch cancelled by app focus loss. Assert that the specified owner receives one terminal outcome and that the release cannot accidentally activate a second object.

### Resolve collision hits into selectable identities

Use an explicit mapping from hit shapes or render IDs to the gameplay selection owner. Filter by interaction capability, visibility policy, and distance before committing. Return a stable handle and generation with query context, rather than retaining an unvalidated component pointer.

**Check** pick a child collider, a compound object, a nonselectable foreground prop, and a selectable object partly behind another surface. Compare the resolved owner with the project's policy. Destroy and reuse the selected object before command execution and verify that the replacement is unaffected.

### Define hover and committed selection separately

Treat hover as a transient query result and committed selection as state owned by the active player or tool. Specify how clicking empty space, switching tools, hiding an item, and changing scene affect that state. Derive outlines and property panels from the same committed identity where they represent selection.

**Check** select an object, move the pointer away, hide it, and switch tools. Verify the outline, properties, and active command target remain consistent with the declared retention rule. Repeat with two local players and confirm their selections stay independent.

### Make ambiguous picks reproducible

Record the camera pose, viewport, pointer coordinate, filters, candidate IDs, rejection reasons, and final owner for a diagnostic capture. For GPU picking, associate the returned ID with the submitted frame and registry revision before resolving it.

**Check** replay a saved camera and pointer fixture with several overlapping objects. Delay the query result until the object registry changes and verify that obsolete results are rejected. Test viewport resizing, different display scale, and the lowest supported input precision.

See [Target selection programming](target-selection-programming.md) for gameplay scoring, [Runtime object highlight programming](runtime-object-highlight-programming.md) for visual requests, and [UI focus navigation programming](ui-focus-navigation-programming.md) for focus ownership.
