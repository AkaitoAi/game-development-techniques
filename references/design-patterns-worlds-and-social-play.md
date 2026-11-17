# Game design patterns for worlds, stories, and social play

## Problem and design reasoning

A player exploring a world needs enough information to choose their own route, infer a story, or coordinate with other people. Too much instruction can remove the discovery those decisions depend on. Too little can turn an open choice into guesswork. Select patterns based on what the player should be able to notice, infer, and do, and decide what happens when they miss the cue or their partner leaves.

Fullbright's developers describe using layout, decoration, and believable spaces to create the intended experience in *Gone Home*. For *Tacoma*, they discuss the specific challenge of making individual story segments non-linear while still tying them to a level. Nintendo's *Breath of the Wild* talk documents the team's efforts to change established series conventions through interacting gameplay rules. *Death Stranding* developers showed at Tokyo Game Show how connecting regions reveals useful traces of other players, such as ladders, and how players can acknowledge the help. A design inference from these cases is to put guidance, story, and cooperation inside the same actions that make the setting playable. [Fullbright, GDC 2015](https://www.gdcvault.com/play/1022112/Level-Design-in-a-Day) · [Fullbright, GDC 2018](https://www.gdcvault.com/play/1025178/Level-Design-Workshop-Designing-for) · [Nintendo, GDC 2017](https://gdcvault.com/play/1024562/Change-and-Constant-Breaking-Conventions) · [Kojima Productions presentation, TGS 2019](https://blog.ja.playstation.com/2019/09/13/20190913-deathstranding/)

## Spatial design and exploration

| Pattern | Problem | Approach and trade-off |
|---|---|---|
| Landmark navigation | Players lose orientation between similar areas. | Give each region a distinctive feature visible from useful approach angles. Test sightlines from ground level. A landmark visible everywhere can make the world feel smaller. |
| Framed vista | Players pass an important location without noticing it. | Frame the destination through an opening or crest while the player retains camera control. The framing weakens when players approach from a different route. |
| Hub and spoke | Several activities need a common point for choosing routes and regrouping. | Connect distinct branches to a readable shared hub with shortcuts where useful. Frequent returns add travel time and can interrupt momentum. |
| Loopback shortcut | Retracing a solved route adds time without another decision. | Let progress open a path back to a known location. The shortcut can erase earlier route choices if it unlocks too soon. |
| Ability gating | A growing move set needs places where old geography becomes newly useful. | Introduce a visible obstacle and let a later ability change how it can be traversed. Players may forget the obstacle before earning the ability. Give them a recognizable cue or a rediscovery route. |
| Soft gating | The player should be able to attempt a route before it becomes comfortable. | Use an observable risk, resource cost, or execution demand to discourage early use. Skilled players may bypass the intended sequence. Design a legal continuation for early arrivals. |
| Multiple routes | A destination has one obvious best route and exploration becomes decorative. | Offer paths with different time, risk, or information costs. A route that is both faster and safer can dominate the alternatives. |
| Breadcrumb trail | The immediate next direction is hard to infer in an unfamiliar space. | Place sparse, readable cues that lead between points where a choice is needed. Dense cues can turn navigation into following a line. |

## Narrative discovery

| Pattern | Problem | Approach and trade-off |
|---|---|---|
| Environmental storytelling | A speech would interrupt play to explain what happened in a space. | Arrange objects so their prior use or changed state implies an event. Essential claims need multiple cues because individual details can be missed or misread. |
| Non-linear clue discovery | Players can find evidence in several orders but the conclusion must remain coherent. | Give each clue enough local context and distribute the required facts across alternative sources. Repeated facts can feel redundant to thorough players. |
| Branch and rejoin | A choice needs consequences without multiplying every later scene. | Let routes differ in information, relationships, resources, or immediate outcomes. Carry those differences through the shared scene. A rejoin feels hollow if all consequences disappear. |
| Delayed consequences | A decision should matter later and immediate scoring would flatten it. | Record the choice and use a later scene, response, or changed place to show the outcome. Give players enough context to connect cause and effect. |
| Optional lore | Players differ in how deeply they want to engage with backstory. | Give the main path its required facts and make deeper context available through optional objects, dialogue, or areas. Keep optional clues legible to interested players without cluttering the critical path. |

## Social play and systemic interaction

| Pattern | Problem | Approach and trade-off |
|---|---|---|
| Complementary roles | Cooperative players perform identical actions independently. | Give roles distinct ways to contribute toward one goal, with flexible handoff or a solo fallback if applicable. Hard role requirements can strand a team when someone leaves. |
| Shared objective and individual contributions | Players understand the win condition but cannot tell how they can help. | Show team progress and recognize distinct meaningful contributions. Individual scoring can encourage actions that harm the team if attribution rewards the wrong behavior. |
| Asynchronous cooperation | Players cannot share a session but can still help each other. | Let actions leave useful artifacts or contributions for later players, and give recipients control over how much appears. Too many artifacts can obscure authored routes or remove intended challenges. |
| Lightweight social signals | Typing or speaking is too slow or inaccessible during play. | Offer a small vocabulary of context-sensitive pings or acknowledgments, accessible alternatives, and mute controls. An ambiguous cue can make receiver and sender act on different assumptions. |
| Public goods | A contribution has a local cost and distributed benefits. | Let players build or maintain a shared resource and show how it helped. If essential infrastructure depends on active contributors, quiet regions may become unplayable. |
| Composable rules | Interactions require a new script for every pair of objects. | Give objects a small set of properties and define consistent rules for their combinations. Each added property increases the number of combinations that must be understood and tested. |
| Systemic chain reaction | Players need to create indirect solutions rather than repeat a prescribed action. | Let one event change a condition that another existing rule observes. Show the major transitions and bound the number of reactions per interval. Rapid chains can be hard to attribute and expensive to simulate. |

## Practical checks

- **Check** run an unbriefed route-finding session with the map UI off. Ask the player where they expect each branch to lead. Record wrong turns, voluntary detours, and whether they can return to the hub from memory.
- **Check** visit the same narrative area in three clue orders, including one path that skips an optional clue. Ask players to state the core conclusion and point to its evidence. Reload with an unresolved branch and verify its prior choice still matters.
- **Check** play a team encounter with an unfilled role, a disconnect, and muted communications. Confirm the remaining players can tell how to contribute and can reach an intended completion or recovery state. Repeat asynchronous play with an empty network and an oversupplied network.
- **Check** combine each new world rule with existing rules in pairs and test the most consequential three-way combinations. Record the trigger, visible outcome, credit, and cost. Confirm a replay or reload does not create an extra reward or preserve an invalid path.

For implementation details, see [Level design](level-design.md), [Environmental storytelling](environmental-storytelling.md), [Quests and narrative](quests-and-narrative.md), [Cooperative gameplay](cooperative-gameplay.md), [Asynchronous multiplayer](asynchronous-multiplayer.md), and [Player communications](player-communications.md).
