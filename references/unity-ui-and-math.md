# Unity UI and visual math workflows

## Problem and design reasoning

UI that depends on one aspect ratio or input device can lose layout and focus in play. Spatial code can look plausible while using the wrong coordinate space. The UI book presents reusable layout and styling in UI Toolkit, and the math book visualizes vectors directly in Unity. Both practices expose relationships that are difficult to inspect through raw values alone. [U10, PDF pp. 72–82](unity-books.md#u10) · [U11, PDF pp. 14, 29–35](unity-books.md#u11)

## Techniques and practical checks

- **Choose UI by feature requirements.** Check the project's actual UI Toolkit or uGUI implementation, Unity version, input integration, and need for world-space UI before choosing a framework. **Check** one representative screen in a player build, including controller navigation, focus return from a modal, and pointer interaction. [U10](unity-books.md#u10)
- **Test reusable layouts with real content.** For UI Toolkit, author hierarchy in UXML, reusable styling in USS, and use flexible sizing where screen width or copy varies. **Check** long translated labels, narrow and wide views, text scaling, and a controller-only path from opening the menu to closing it. [U10, PDF pp. 72–82](unity-books.md#u10)
- **Draw vectors before changing code.** Use Gizmos or controlled debug visualization to show the source, target, facing direction, and comparison angle. For two nonzero unit vectors in the same space, use `Vector3.Dot(a, b) >= Mathf.Cos(angleDegrees * Mathf.Deg2Rad)` for the inclusive cone boundary. **Check** straight ahead, behind, just inside and outside the boundary, zero-length input, and a rotated parent. [U11, PDF pp. 8–22, 29–35](unity-books.md#u11)
- **Test rotation in the space that owns it.** Decide whether a change is relative to the object, its parent, or world axes. Use quaternions for composition and interpolation and keep degrees and radians explicit at API boundaries. **Check** with a rotated parent and a target crossing the angle wrap. Visualize basis vectors and verify the intended axis and travel direction. [U11, Chapter 3](unity-books.md#u11)
