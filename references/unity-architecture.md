# Unity architecture and authoring

## Problem and design reasoning

Direct scene-object dependencies make reusable prefabs hard to instantiate and scenes hard to test independently. Unity's ScriptableObject guide uses project assets as event channels to decouple publishers from listeners across scene boundaries. Use that seam when independent modules must communicate and designers benefit from Inspector wiring. A local component reference is simpler when both ends share a lifetime. [U1, PDF pp. 48–55](unity-books.md#u1)

## Techniques and practical checks

- **Separate authored data from session state.** Store shared tuning in ScriptableObject assets and create per-session or per-instance state in a runtime owner. **Check** two characters using one definition asset. Change one character's health, reload the scene, and confirm the shared definition and the second character stay intact. [U1, PDF pp. 4–10](unity-books.md#u1)
- **Keep event lifetimes visible.** Pair subscription and unsubscription with the listener's active lifetime. Document which asset carries the event and which component owns any state it changes. **Check** disable and re-enable an object, reload additively, and raise the event once. Verify one response per live subscriber. [U1, PDF pp. 48–55](unity-books.md#u1)
- **Pool only measured churn.** Use an object pool for frequent, temporary objects if captures show allocation or construction cost. Reset gameplay state, timers, and presentation on reuse. **Check** a burst above the normal active count and repeated returns to the pool. Compare allocation and worst-frame time against the baseline. [U3, PDF pp. 45–51](unity-books.md#u3)
- **Use prefab variants for shared structure.** Give one base prefab common behavior and author deliberate differences in variants. **Check** changing a base collider and an overridden variant parameter. Inspect which changes propagate and run the prefab in a scene without editor-only dependencies. [U9, PDF pp. 34–36](unity-books.md#u9)
