# Game resource residency

## Problem and design reasoning

A valid resource handle can outlive the backing memory it refers to. A resident resource occupies the relevant memory pool and is ready for its consumer. Making every known resource resident can exceed budget, while repeated eviction and reload can cause stalls. Distinguish allocation, residency, and retirement so the policy can preserve useful data while respecting work already submitted.

In shipped *RAGE*, id Software used software virtual textures to support unique surface detail without keeping the entire texture address space in physical memory. **Design inference:** stable logical addresses and predictable fallback data let an engine trade resident detail for performance under pressure. On explicit graphics APIs such as Direct3D 12, residency also has a synchronization requirement because the GPU must be able to access referenced memory when its work executes. [J.M.P. van Waveren, Software Virtual Textures, id Software](https://www.mrelusive.com/publications/papers/Software-Virtual-Textures.pdf) · [Microsoft, Direct3D 12 residency guidance](https://learn.microsoft.com/en-us/windows/win32/direct3d12/residency)

## Practical checks

- **Check** set an artificially low pool budget and move between distant views. Verify that pinned and recently useful resources take priority, a fallback remains available, and staging memory counts against peak usage.
- **Check** alternate rapidly between two views that cannot both fit at full detail. Record bytes loaded and evicted per second. Confirm a grace period or a reduced-quality policy bounds churn.
- **Check** delay both compute and graphics completion while eviction has been requested. Verify the engine tracks each queue's last use and reuses backing memory only after all required completions.
- **Check** issue a load, immediately release interest, and request the same key again before the first result arrives. Verify generation or equivalent identity checks reject stale completion and that resident accounting returns to baseline after retirement.
