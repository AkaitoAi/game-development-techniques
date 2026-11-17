# Player photography as a game mechanic

## Problem and design reasoning

When taking a photo advances a task, the player needs to understand what evidence the game accepts and still have room to choose a subject, angle, or moment. Watch for players who take what looks like a valid shot and cannot explain why it failed. Also watch for players who can finish every task by photographing indiscriminately.

Naphtali Faulkner described photography in *Umurangi Generation* as a way to make players examine what the environment means to its inhabitants. The team introduced controls gradually and reserved more advanced options for players who pursued extra objectives. A transferable inference is that completion rules and creative tools can be tuned independently. [Developer interview](https://www.gamedeveloper.com/design/using-photography-to-document-the-end-of-the-world-in-i-umurangi-generation-i-) · [LudoNarraCon 2021 developer panel](https://www.youtube.com/watch?v=dslrYYN40kM)

## Shipped examples

- *New Pokémon Snap* evaluates expedition photos and uses the selected results to build a Photodex. This gives a repeatable route an observation objective and lets players revisit it to improve their collection. [The Pokémon Company release](https://press.pokemon.com/en/NEW-POKEMON-SNAP-AVAILABLE-NOW) · [Create Your Own Photodex](https://newpokemonsnap.pokemon.com/en-ca/create-photodex/)
- *Umurangi Generation* asks players to complete photo bounties, gives them additional camera equipment to unlock, and supports creative choices beyond the required subjects. [Developer's Steam listing](https://store.steampowered.com/app/1223500/Umurangi_Generation/) · [Developer interview](https://www.gamedeveloper.com/design/using-photography-to-document-the-end-of-the-world-in-i-umurangi-generation-i-)
- The creators of *TOEM* report that prototyping a camera and album gave players a reason to slow down and look closely at the art. They developed its photography into the main way to help characters and explore. [Lucas Gullbo and Niklas Mikkelsen's postmortem](https://www.gamedeveloper.com/production/postmortem-toem) · [Studio game page](https://www.somethingwemade.se/toem/)

## Practical techniques

1. Write each objective as a rule over game state. Identify the required subject or relationship, permitted actor states, visible area, and framing tolerance. Keep required evidence distinct from optional aesthetic rewards. **Check** a valid shot from two different angles, a shot with the subject behind an obstruction, and a shot of a similar-looking subject. Verify that only the valid shots fulfill the objective.
2. Evaluate against the captured frame. Bind the camera transform, projection, timestamp, actor identifiers, and relevant visibility or pose results to the same capture. If evaluating asynchronously, preserve the capture-time state rather than looking up the current scene. **Check** a shot during a fast pan and another as a subject changes pose or despawns. Compare the stored image with the scoring input.
3. Give feedback players can use. Show which objective or subject was recognized and a useful reason for a near miss, such as the subject being too small or cut off. Use a forgiving introductory objective before adding simultaneous conditions or rare poses. **Check** with first-time players who deliberately miss twice. Ask whether they can explain the result and change their next attempt without coaching.
4. Author repeatable opportunities. Give optional creative moments room to stay surprising and give required objectives a reliable way to try again. Keep album persistence independent from temporary scene objects. **Check** the same task after a missed encounter, reload, and revisit. Verify the subject can be found again, album entries still display, and resubmission follows the intended progression rule.
