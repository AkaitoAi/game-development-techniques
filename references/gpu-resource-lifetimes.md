# GPU resource lifetimes

## Problem and design reasoning

GPU work can outlive the CPU object that submitted it. Returning a buffer, its descriptor slot, or its backing storage to an allocator too early can affect an older submission. Holding every allocation until shutdown avoids reuse hazards but wastes memory. Give each resource a logical owner and track the completion points for every queue that uses it.

- Graham Wihlidal's GDC 2017 presentation on checkerboard rendering in *Battlefield 1* and *Mass Effect: Andromeda* identifies render targets as a major part of the 4K memory increase. The talk reports that explicit render-target aliasing saved approximately 230 MB. [GDC slides](https://media.gdcvault.com/gdc2017/Presentations/Wihlidal_4kCheckerboardIn.pdf)
- AMD's engineering write-up on *DOOM*'s Vulkan work discusses pooling smaller resources in larger allocations and the coordination required when images transfer ownership between GPU queues. [AMD GPUOpen](https://gpuopen.com/learn/vulkan-and-doom/)

These examples suggest separating backing allocation policy from object retirement. Pooling helps with allocation overhead and memory budgets. A completion rule handles whether a particular allocation can be reused.

## Implementation and checks

- Stamp each submission with a monotonically increasing completion value on its queue. When an owner releases a resource, retire its object, referenced descriptor slots, and backing range once every recorded queue value is complete. **Check** delay copy or compute work across multiple frames, replace a streamed texture, and verify its old descriptor and backing range cannot be recycled until the delayed submission completes.
- Use a fenced ring or per-frame pages for upload staging. Reuse an upload range after its consuming GPU work completes. Set a budget and an explicit overflow policy. **Check** stream a large area with deliberately increased GPU latency. Exercise ring wraparound and confirm that data remains intact and staged bytes stay within the chosen limit.
- Alias compatible temporary resources whose actual GPU accesses are ordered and whose lifetimes cannot overlap. Record the resource-state transition or aliasing barrier required by the backend. **Check** compare captures with aliasing enabled and disabled, then toggle asynchronous passes and dynamic resolution to verify the lifetime assumptions and peak memory savings.
- Rebuild size-dependent resources when presentation changes. Retire the previous generation using the backend's completion rules and rebind dependent views to the new generation. **Check** resize repeatedly while transfers and frames are in flight. Verify there are no validation errors and that memory returns to the expected steady-state range after completion.
