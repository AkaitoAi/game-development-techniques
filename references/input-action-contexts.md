# Input action contexts

## Problem and design reasoning

The same physical control can mean jump during play, confirm in a menu, and accelerate in a vehicle. Polling it independently in each system can activate several actions or leave an old action held after a mode change. Valve's shipped *Portal 2* gained native Steam Controller actions and automatically updated button prompts. Later notes record fixes for selecting the correct action set when switching between gameplay and Puzzle Maker. This is evidence that mode transitions are part of input correctness. [Valve release notes](https://store.steampowered.com/news/posts/?appids=620&enddate=1643751700&feed=steam_community_announcements)

Valve's Steam Dev Days 2016 presentation explains action-based integration. Steam Input documents action-set layers that add or override actions. The portable recommendation is to give one per-player context resolver responsibility for active actions, then publish semantic actions to gameplay. The specific ownership protocol below is an implementation recommendation. [Valve, Steam Controller, Steam Dev Days 2016](https://cdn.akamai.steamstatic.com/apps/steamdevdays/slides2016/SteamController.pdf) · [Valve action-set layers](https://partner.steamgames.com/doc/features/steam_controller/action_set_layers?l=english)

## Techniques and practical checks

- Give contexts explicit priority and ownership. Represent a menu, vehicle seat, or aiming layer with a lifetime token and define which actions it replaces or passes through. **Check** open a modal while aiming and close it while a movement control remains held. Verify only the intended owner receives each action and focus returns to the correct player.
- Resolve changes at a documented input boundary. Decide whether an activation event can reach a newly opened context and how blocked held controls become eligible again. **Check** use one button to open a menu that uses the same button to confirm. Verify that the opening press cannot accidentally select the default item.
- Keep logical actions independent of bindings and glyphs. Use the resolved context and active device to select prompts from the same action definitions that receive input. **Check** switch between controller and keyboard in a vehicle, a modal, and ordinary play. Compare each displayed prompt with the action that actually executes.
- Trace context changes with player ID, owner, active actions, and reason. Remove all layers owned by a scene or disconnected player when that owner exits. **Check** interrupt a layered interaction with scene unload, controller removal, and player respawn. Verify the new context contains no orphaned layers or stuck actions.

See [Input rebinding](input-rebinding.md) for binding capture and [UI focus navigation programming](ui-focus-navigation-programming.md) for focus within the selected UI context.
