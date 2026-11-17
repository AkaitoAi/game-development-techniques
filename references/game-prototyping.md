# Game prototyping and vertical slices

## Problem and design reasoning

A polished demo can hide an untested mechanic or an unsustainable production process. A mechanic prototype answers a narrow question about play. A vertical slice answers whether a representative section can reach the intended experience through the production pipeline. Decide which uncertainty is holding up the next commitment and build enough to resolve it.

## Shipped examples

**The Legend of Zelda: Breath of the Wild (2017).** Nintendo demonstrated a top-down prototype used to explore interactions that would become part of its 3D game. The developers presented the test in the context of designing play around combinations of objects and rules. The practical inference is that a simpler representation can isolate an interaction if it preserves the relationships being tested. [Hidemaro Fujibayashi, Takuhiro Dohta, and Satoru Takizawa, GDC 2017](https://www.gdcvault.com/play/1024562/)

**Firewatch (2016).** Campo Santo showed about 20 playable minutes of the opening at GDC, polished to the level envisioned for release. The team reported that other sections were still unfinished and highlighted the need for playtesting to find interactions players expected during exploration. The inference is that the opening established a visible quality target, while production repeatability and breadth of reactivity needed additional evidence. [Campo Santo Quarterly Review, Vol. 2 Q1](https://www.camposanto.com/quarterlyreview/assets/Campo%20Santo%20Quarterly%20Review%20Volume%202%20Q1.pdf)

## Techniques and practical checks

### Convert a design assumption into a test

State the assumed player behavior, the interaction to build, and the result that would make the team revise or drop it. For example, test whether a player can infer that moving a crate changes the path of flowing water. Include one opportunity to apply the same rule in a new arrangement.

**Check** what unbriefed players try first and whether they transfer the rule. Record facilitator interventions and distinguish success through understanding from success through random attempts.

### Choose fidelity around the question

Use stand-in art and a tightly scoped level for evaluating rules. Add the actual camera, latency, collision, or sound when those properties are part of the hypothesis. List which assumptions the prototype leaves untested so its results survive handoff accurately.

**Check** an interaction in both the simplified fixture and a short production-like view. Compare recognition, execution time, and failure reasons before investing in more content.

### Make the slice exercise the content pipeline

Select a short representative sequence with both an ordinary case and a difficult one. Include the authoring and integration steps that have to be repeated across the game. Define quality, performance, and production-time criteria before the slice is evaluated.

**Check** by having another developer build or modify a second equivalent section with the same tools. Track elapsed time, manual fixes, recurring bugs, and packaged-build performance.

### Decide which work graduates

Record which hypotheses survived and which tools, assets, or code have known production costs. Keep the validated design behavior as acceptance criteria. Review prototype code against integration, state ownership, testability, and performance requirements before carrying it into production.

**Check** that the production implementation reproduces the accepted behavior under target conditions and that the slice has a named follow-up for every critical untested assumption.
