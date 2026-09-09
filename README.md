# Ridge Sprint

A single-file, dependency-free pseudo-3D arcade racer that runs in any modern browser. One HTML file, no build step, no assets — the road, cars, and scenery are all drawn procedurally on a `<canvas>`.

## Features

- **Pseudo-3D road engine** — segment projection with curves, hills, rumble strips, and lane markings
- **4 playable cars** with distinct handling (accel / top speed / grip multipliers)
- **3 difficulty levels** — Rookie, Normal, Pro (timer, traffic density, fast-car share, grip, and top speed all scale)
- **3 lives** — each collision costs a heart, with a short blinking recovery window; lose all three and the run ends early
- **High scores** — top-5 leaderboard plus per-difficulty bests, saved in `localStorage`
- **Living roadside** — pines, oaks, bushes, marker posts, road signs, billboards, windmills, a start gantry, and drifting clouds
- **Traffic** — cruisers and low-slung speedsters that weave between lanes
- **Keyboard + touch controls**, responsive layout, and on-screen pads on small screens

## Getting started

Clone the repo and open the file — that's it.

```bash
git clone https://github.com/<you>/ridge-sprint.git
cd ridge-sprint
open ridge-sprint.html      # macOS
# or: xdg-open ridge-sprint.html   (Linux)
# or just drag the file into your browser
```

Optionally serve it locally:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000/ridge-sprint.html
```

It is a static file, so it also drops straight into GitHub Pages, Netlify, or Vercel.

## Controls

| Action | Keys | Touch |
| --- | --- | --- |
| Accelerate | `↑` / `W` | GAS pad |
| Brake | `↓` / `S` | BRAKE pad |
| Steer | `←` `→` / `A` `D` | ◀ ▶ pads |
| Start / restart | `Space` / `R` | Start race button |
| Pick difficulty (in menu) | `1` `2` `3` | Difficulty buttons |

## Cars

| Car | Style | Accel | Top speed | Grip |
| --- | --- | --- | --- | --- |
| Comet GT | Open-wheel formula | 1.00 | 1.00 | 1.00 |
| Bolt X | Low wedge | 0.90 | 1.12 | 0.90 |
| Boulder 4x4 | Truck | 0.85 | 0.92 | 1.22 |
| Vesper R | Coupe | 1.18 | 0.96 | 1.06 |

Car multipliers stack with difficulty settings, so Bolt X on Pro is the fastest and twitchiest combination.

## Difficulty

| Level | Timer | Traffic | Fast cars | Grip | Top speed |
| --- | --- | --- | --- | --- | --- |
| Rookie | 75s | 26 | 10% | 1.15× | 0.92× |
| Normal | 60s | 44 | 28% | 1.00× | 1.00× |
| Pro | 50s | 66 | 48% | 0.86× | 1.08× |

## Scoring

You score the distance covered before the clock runs out. Crashes cost a life and a large chunk of speed, so clean lines matter more than raw throttle. Runs end early if all three lives are gone.

Saved keys in `localStorage`:

- `ridgeSprintScores` — top-5 leaderboard (distance, car, difficulty)
- `ridgeSprintBest_<difficulty>` — best distance per difficulty
- `ridgeSprintCar`, `ridgeSprintDiff` — last selections

Clear them from your browser devtools to reset progress.

## Project structure

```
ridge-sprint.html    # everything: markup, styles, and game code
README.md
```

Inside the file:

- **Track building** — `buildTrack`, `addRoad`, `addSegment`, `addScenery`
- **Traffic** — `buildTraffic`, collision handling in `update`
- **Projection & drawing** — `project`, `render`, `renderSegment`, `drawBackground`, `drawSprite`, `drawCar`, `drawPlayer`
- **Game state** — `startRace`, `finish`, `setCar`, `setDifficulty`, `updateHud`, `renderScores`

## Performance notes

- Traffic is bucketed by track segment once per frame instead of being scanned per drawn segment
- Off-screen, sub-pixel, and below-viewport sprites and cars are culled before drawing
- The sky gradient is built once at startup
- HUD elements are only written when their value changes

Tweak `drawDistance`, `segmentLength`, and the per-difficulty `traffic` counts near the top of the script to trade visual range for frame rate on low-end devices.

## Browser support

Any browser with `<canvas>`, `requestAnimationFrame`, and `localStorage` — current Chrome, Firefox, Safari, and Edge. Touch controls appear automatically on viewports under 640px.

## License

MIT. Do whatever you like with it.
