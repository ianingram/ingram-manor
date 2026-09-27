# Ingram Manor — session handoff

Read this first. It replaces scrolling back through the last session.

## What this is

One file, `index.html`, 229 kB, 4,779 lines. A Three.js scene: five hovering
craft over water, the Great Pyramid behind them, a real sky computed from the
viewer's location and clock. Live at `ianingram.github.io/ingram-manor/`,
linked from the GannLab desk.

Everything is procedural — every texture is drawn to canvas at load, no assets.
Startup is ~310 ms and the frame costs ~2.8 ms of JavaScript.

## Live

| | |
|---|---|
| Repo | `ianingram/ingram-manor` |
| Page | `ianingram.github.io/ingram-manor/` |
| File | `index.html` (the whole thing) |
| Three.js | 0.160 via import map |

## The systems

**Astronomy.** Sun, moon and six planets from orbital elements, Kepler solved by
iteration. Moon distance computed (369,000–404,000 km across a month), not the
mean. 900 stars on a sphere turning at sidereal rate. The check that matters:
every planet lands within a few degrees of the ecliptic *without being told to* —
it falls out of the elements. Location from time zone, refined by geolocation.

**The sky.** Ecliptic in gold and the galactic plane in blue, crossing at 60.2°
(the real figure — get the galactic pole wrong and that number goes wrong).
Polaris, Vega and Draco as named anchors. Two scrolling cloud decks overhead —
planes with tiling textures and scrolled offsets, exactly as the sea works —
with a radial alpha ramp that thins them overhead and dissolves the outer rim.
Cover runs a five-minute cycle, the high deck lagging the low one by 42 s.

**The sea.** Normal-mapped water with reflections of the fleet, a sun-glitter
track, and a vortex system: a spiral decal plus 90 airborne mist puffs that
orbit the eye, fired on takeoff and landing.

**The pyramid.** Gold veins in the stone, three passes of gold edging, one
rotation every four minutes. A **depth-only mask** (`pyMask`, `colorWrite:false`,
render order −40) makes it occlude solid things behind it while the sky still
shows through — without it, a craft passing behind drew over the stone and
looked like it was inside it.

**The fleet.** Five craft, 107 objects each. Intake with a glowing ring that runs
white hot, two exhaust nozzles throwing plasma, keel thrusters, sails with drawn
(not typed) emblems, and three sets of lights: navigation always on, amber
takeoff strobes during spin-up, white landing lamps blinking below 500 ft with a
pool thrown on the water.

**The patrol.** Craft X leaves formation every 46 s of rest. See below.

**The Giza clock.** Probe 41's construction, live: a body's rising azimuth
selects one of 210 courses, lit as a band on the pyramid with a ray on the
bearing. Reads Jupiter when it's up, else the highest of Mars, Saturn, Venus,
Mercury, Moon.

**The Jump Control Surface.** Right-edge rail, collapses to a `JCS` tab. Prices a
jump to every body as spool seconds — range, occlusion, off-axis angle, per-craft
drive coefficient — scans the next 24 hours for the cheapest window, and counts
down to it. Nothing is ever refused; it only costs more. Glyph strip along the
bottom, each body in its own hue.

**The moon.** Jump to lunar orbit, then land: cratered regolith, black sky, hard
white sun fixed in place (a lunar day is a month — it does not follow the clock
at home), Earth in the sky, the same pyramid in bright stone.

## The patrol, in order

| stage | seconds | what |
|---|---|---|
| Launch | 12 | Water moves first: three seconds of vortex with the engines dark and the craft trembling on station. Then the drive lights, wind rises, craft strains. Then it lifts — **level, never nose-up** — while the intake runs to white. Sea stirred five times across the twelve. |
| Slide | 2 | Holds altitude, slides left, comes round onto the bearing. Plasma still cold. |
| Flight | 92 | Plasma lights once it is >34 units from every other craft. Climb 23°, hold at 620, descend −20°. Boosters fire when it comes level: seven rings that expand, stall and hang nine seconds. |
| Approach | 10 | One spline: long turn onto the line, short final, flare. Heights fall the whole way. Landing lamps blinking, water turning beneath. |

**The trajectory is defined by formula, not waypoints** (`patrolPath`). Height is
a smoothed trapezoid; the ground track is one continuous arc; two Gaussian nudges
give the pyramid a berth. This matters — see below.

## What cost the most time, and why

Five bugs ate most of the session. None was a tuning problem and none was
visible from the outside.

1. **The craft vanished at distance.** The camera's far plane was 2,200 and the
   craft flies to 2,900. It was being clipped out of existence. I spent several
   rounds tuning the marker's brightness and size while the object was outside
   the view volume. *Check the frustum first.*

2. **The trajectory looked like a stock chart.** A spline through twenty
   hand-placed points ripples between them. Every fix moved a point and produced
   a new wobble elsewhere. *Define trajectories by formula and sample them.*

3. **It looked like it was flying through the pyramid.** It never touched it. The
   stone is translucent, so passing in *front* is indistinguishable from passing
   *through*. I kept testing collision when the right test was screen projection
   — and then fixed it properly with the depth mask.

4. **The level dash looked like hovering.** It was perfectly level. It ran
   straight away from the camera, and motion directly away projects to nothing.
   Then the camera was tracking it, so it stayed centred. Tracking is now split:
   follow in elevation, let go in bearing.

5. **The boost was a teleport.** It added 0.00085 to the path position per frame
   for five seconds — a quarter of the whole route. The craft levelled out, lit
   the boosters, and skipped the entire dash.

Two rules worth keeping: **when a fix does not hold twice, the model is wrong,
not the numbers**; and **measure, do not look** — nearly every fix here came from
a probe rather than from reasoning about it.

## Open

1. **1,000 draw calls.** 67k vertices is nothing, but 1,000 meshes and 471
   materials is a lot for a Late-2014 iMac. This is the one number never tested
   on real hardware. If the page feels heavy, merge the static ship geometry per
   material — do not cut features first.
2. Header comment still says "three caravels". Cosmetic.
3. Sky-dot views 2 and 3 (top-left) are thin — view 2 is a sunset preset, view 3
   is empty.
4. Ships other than X never patrol. The machinery is general; only X is wired.

## Testing

No test runner. Probes were written per question as standalone Node scripts that
load the page's script into a stubbed DOM with a fake WebGL renderer, step
`requestAnimationFrame` by hand, and assert on real numbers — path clearance,
climb angles, screen projection, light states, frame cost. Real WebGL renders
via `xvfb-run` are possible but slow (2–4 minutes); the numeric probes take
seconds and caught everything that mattered.

Nothing is minified. Read the comments — the reasoning behind the awkward parts
is written where the code is.
