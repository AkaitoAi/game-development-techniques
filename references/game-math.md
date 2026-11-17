# Mathematics for games

## Problem and design reasoning

**Problem.** Movement changes under different frame rates, coordinate-space conversions disagree, or identical inputs diverge across machines.

**Design reasoning.** Psyonix describes a fixed tick and correction replay in Rocket League. Wube's desync investigation shows why configuration and execution matter in addition to seeded randomness. The practical inference is to declare units, coordinate frames, and numerical tolerance before selecting a formula.

Use this reference when a feature needs a geometric test, a coordinate conversion, a time-dependent response, a random distribution, or a numerical tolerance. Define the coordinate space, units, and valid input range before choosing a formula.

## Shipped examples

- **Quake (1996).** The released WinQuake source turns yaw, pitch, and roll into forward, right, and up vectors using sine and cosine. It also classifies an axis-aligned box against a plane using dot products at its extreme corners. These operations still fit camera-relative controls and spatial queries. [id Software, WinQuake math source](https://github.com/id-Software/Quake/blob/master/WinQuake/mathlib.c#L150-L299)
- **Rocket League (2015).** Psyonix describes a fixed-rate physics simulation and client-side corrections that replay inputs from a known simulation frame. Rendering or smoothing can operate on a separate clock while the simulation computes authoritative results at discrete times. [Jared Cone, It IS Rocket Science!, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Cone_Jared_It_Is_Rocket.pdf)
- **Factorio (2020 onward).** Wube documents a deterministic multiplayer simulation and exposes a separately seedable deterministic random generator in its scripting API. Its debugging notes describe a desync that needed machines with different CPU core counts to reproduce. The two sources illustrate different requirements for reproducibility: control over random state and stable execution across supported environments. [Wube, multiplayer architecture](https://www.factorio.com/blog/post/fff-302) · [Wube, random generator API](https://lua-api.factorio.com/latest/classes/LuaRandomGenerator.html) · [Wube, Friday Facts #415](https://www.factorio.com/blog/post/fff-415)

## Techniques and practical checks

### Use vectors for distance, facing, slope, and motion

Between points `A` and `B`, the displacement is `d = B - A`. Compare squared distance `dot(d,d)` to `radius²` when only a range test is needed. Facing lies inside a cone when `dot(unitForward, unitD) >= cos(halfAngle)`. Define behavior when the target coincides with the origin, where `unitD` is undefined. Given a unit surface normal `n`, compute the tangent component of motion with `v - dot(v,n)*n`. For camera-relative ground motion, project the camera's forward axis onto the ground plane and construct the perpendicular horizontal axis with a cross product. Use a fallback heading when the forward projection is nearly zero.

**Check** a target directly ahead, at the boundary, behind, and at the same position. For a flat floor with `n = (0,1,0)`, projecting `v = (3,-2,4)` should give `(3,0,4)`. Repeat with a slope and a nearly vertical camera.

### Transform points, directions, and normals correctly

Using column-vector notation, `p_world = M * (p_local, 1)` and `d_world = M * (d_local, 0)` for an affine transform `M`. The direction receives the linear transform without translation. For a surface normal, use `normalize(inverse(transpose(A)) * n_local)`, where `A` is the invertible linear part of `M`. This matters under nonuniform scale. With a hierarchy, `M_world = M_parent_world * M_local` in this convention. To recover a local point, apply `inverse(M_world)`. Check the engine's row or column convention, handedness, axis definitions, and angle units.

**Check** by transforming a child point under a translated, rotated, and nonuniformly scaled parent, then converting it back. Verify that the transformed normal is perpendicular to both transformed surface tangents. Test mirrored transforms and define behavior for singular scales before using an inverse.

### Use shortest-path rotation where appropriate

For a wrapped heading in degrees, use `delta = positiveModulo(target - current + 180, 360) - 180`, with a project-defined tie rule at exactly 180 degrees. For 3D orientations, interpolate unit quaternions `a` and `b` with spherical interpolation. Negate `b` when `dot(a,b) < 0` to select the short arc, and use normalized linear interpolation as a near-parallel fallback. Keep authored spins as explicit motion when their winding is part of the effect.

**Check** a heading change from 359 to 1 degrees and the reverse. Verify that interpolating equivalent orientations `q` and `-q` holds the pose. Test a 180-degree transition for repeatable arc choice and a deliberately authored full turn for preserved winding.

### Choose interpolation by timing intent

Use `lerp(a,b,t) = a + (b-a)*t` when `t` describes progress between known endpoints. Clamp `t` when extrapolation is unwanted. For a smoothed value following a target, use `x_next = x + (target-x)*(1 - exp(-k*dt))`, with `k = ln(2)/halfLife` and positive `halfLife` in the same time units as `dt`. This yields the same response after equal elapsed time for a constant target. For rendering between neighboring simulation states, compute the fraction from their timestamps and keep the result out of authoritative gameplay state.

**Check** a stationary target with 30, 60, and 144 FPS step sequences. Starting at 0 and following 1, expect 0.5 after one half-life and 0.75 after two. Then try a target teleport and confirm the intended reset or smoothing behavior.

### Make probability and random state explicit

For weights `w_i >= 0` with total `W > 0`, the probability of item `i` is `w_i/W`. Select the first cumulative weight greater than a uniform draw in `[0,W)`. Use an unbiased bounded random draw for integer weights. For independent rolls with probability `p`, the chance of at least one success after `n` rolls is `1 - (1-p)^n`. A pity timer, shuffled bag, or changing weight breaks the constant independent-roll assumption. Keep authoritative random streams separate from presentation and capture their state for saves or replays when reproducibility matters.

**Check** an empty table, all-zero weights, zero-weight entries, interval boundaries, and a known seeded sequence. For an example 1:3 table, check that repeated sampling stays near 25% and 75% using a tolerance based on sample size. For an independent binomial count, the expected count is `n*p` and its standard deviation is `sqrt(n*p*(1-p))`.

### Set numerical tolerances from the game scale

For finite values, a useful approximate comparison is `abs(a-b) <= max(absTol, relTol*max(abs(a),abs(b)))`. Set `absTol` in the feature's units and `relTol` for the supported magnitude range. For positive normal binary32 numbers in `[2^e,2^(e+1))`, adjacent representable values are spaced by `2^(e-23)`. At 1,000,000 units this is 0.0625 units. If nearby motion requires finer resolution, keep a higher-precision world position and convert to local or camera-relative floats after subtracting the origin. Keep exact identity and integer counters outside tolerance comparisons.

**Check** a small displacement at the origin and again near the farthest supported coordinate. Verify motion, ray queries, and camera stability around an origin shift. For deterministic systems, replay the same input and random state under supported builds and thread counts and locate the first divergent tick.
