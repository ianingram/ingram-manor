# ingram-manor

The front door for Ingram Manor LLC — a single page showing the four projects
as a fleet of craft standing off a pyramid, under a sky computed from wherever
and whenever you happen to be looking at it.

**Live:** [ianingram.github.io/ingram-manor](https://ianingram.github.io/ingram-manor/)

## Looking at it

Open `index.html`. That is the whole thing — one file, no build step, no
dependencies beyond Three.js from a CDN, and not a single image asset. Every
texture in the scene is drawn to a canvas when the page loads.

- **Drag** to look around — the full compass, and up to the zenith.
- **Click a craft** to read what it is.
- **JCS**, bottom right, opens the Jump Control Surface.
- The **three dots**, top left, switch between views.

## The fleet

Each craft carries one project on its sail.

| | |
|---|---|
| ☥ Amenti Interface | the platform |
| ⊕ GannLab | the research |
| ♞ Treasure Chess | the game |
| ✕ — | unannounced |
| ☉ Amenti.live | live |

## The sky is real

This is the part worth knowing. The sky is not a backdrop — it is computed
from your time zone, refined by geolocation if you allow it, and from the
clock on your machine.

- **Sun and moon** in the right place, with the moon's true phase and its real
  distance, which swings from 369,000 to 404,000 km across a month.
- **Six planets** from orbital elements, Kepler solved by iteration. The proof
  it is real: they all sit within a few degrees of the ecliptic without being
  told to. That falls out of the mathematics.
- **900 stars** turning at sidereal rate, with Polaris a degree off the pole
  and Vega 51° away — where the pole will point around 13,700 AD.
- **The ecliptic and the galactic plane**, crossing at 60.2°, which is the
  real figure.

So Jupiter is under the horizon when Jupiter is under the horizon, and the
whole thing looks different at four in the morning than it does at noon.

## The instruments

**The Giza clock.** The pyramid keeps time. A body's rising azimuth selects one
of its 210 courses, lit as a band around the stone with a ray on the bearing.
It reads Jupiter when Jupiter is up, and the next brightest thing when it is
not. This construction was tested and retired as a trading signal — it
correlates with price at r = 0.021 — and is kept here as what it honestly is,
which is a clock.

**The Jump Control Surface.** Prices a jump to every body in the sky as spool
seconds: real distance, how far under the horizon it is, how far off your aim,
and the craft's own drive. It scans the next twenty-four hours for the cheapest
moment and counts down to it. Nothing is ever refused — going early simply
costs more. Lock on, wait for the window, and go. You can reach lunar orbit and
land.

## Built with

Three.js 0.160. No framework, no bundler, no assets. Written in a browser.

---

Ingram Manor LLC. 2026. All rights reserved.
