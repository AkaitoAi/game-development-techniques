# UI list virtualization

## Problem and design reasoning

A large inventory, server browser, or quest log can create hundreds of live rows even though only a handful are visible. The cost then grows with the data set's size. In his GDC 2015 talk, Yee Cheng Chin describes the shipped HTML/CSS/JavaScript UI in *SimCity* (2013), including memory bloat and integration efficiency as engineering concerns. This establishes the cost of a complex game interface, rather than evidence that *SimCity* used the exact virtualization protocol below. [Maxis, How to Implement AAA Game UI in HTML and JavaScript](https://www.gdcvault.com/play/1022054/How-to-Implement-AAA-Game)

Epic documents a concrete virtualized list model that distinguishes the collection's items from the smaller number of row widgets used to present them. A portable implementation can use stable item IDs and recycled rows so visual cost follows the visible range. The ownership and cancellation rules below are implementation recommendations. [Epic, ListView API](https://dev.epicgames.com/documentation/en-us/unreal-engine/python-api/class/ListView?application_version=5.3)

## Programming techniques and practical checks

### Keep item identity independent of rows

Store selection, expansion, and item actions against stable data IDs. Bind a row to an ID only while it represents that item. On release, clear item subscriptions, pointer captures, animations, and item-specific callbacks. On rebinding, reset all visual state before showing the new data.

**Check** scroll until the selected item's row is recycled, then scroll back. Sort, remove, and insert entries while a row is hovered or pressed. Confirm that selection follows the intended item and a click acts on the currently bound ID.

### Bound row creation around the viewport

Calculate the visible item range from scroll offset and row extents, then include a small overscan region. For variable-height content, maintain measured extents and a predictable scroll anchor when measurements change. Pool enough rows for the largest supported viewport and retire excess capacity according to the menu's memory budget.

**Check** lists of 20, 2,000, and 20,000 items at the same viewport size. Record live row count, allocations, and frame time while jumping to the end and dragging the scrollbar. Verify that live rows remain bounded by viewport demand.

### Version asynchronous row work

Tag an icon load or delayed tooltip with the item ID and row binding generation. Apply completion only when both still match. Release any result the row no longer owns. Keep loaded icon caching separate from row lifetime so recycling does not force redundant loads.

**Check** delay icon completion, scroll rapidly, close the screen, and reopen it with another collection. Confirm that late callbacks cannot replace a different item's icon or modify a retired row.

### Preserve navigation through data changes

Treat focus as a logical item and a visible row as its temporary presentation. Scroll the logical focus into view before assigning physical focus. Choose a fallback when filtering removes that item, and restore the scroll anchor after content changes where practical.

**Check** navigate using a controller while filtering, changing language, and resizing the view. Verify one focused item, a valid visible focus target, and consistent accessibility labels after row reuse.

See [UI focus navigation programming](ui-focus-navigation-programming.md) for focus ownership and [Runtime text layout and font caches](runtime-text-layout-and-font-caches.md) for variable text extents.
