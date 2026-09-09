# Ridge Sprint

A little arcade racing game that lives in one HTML file. No build step, no npm install, no image assets. The road, the traffic, the trees and the clouds are all drawn on a canvas at runtime, so you can just double click the file and drive.

I built it as a small pseudo 3D experiment (the classic segment projection trick used by old Out Run style games) and then kept adding to it until it felt like an actual game.

## What's in it

* A curving, hilly road with rumble strips and lane markings
* Four cars that genuinely feel different, not just recolours
* Three difficulty levels that change the timer, traffic, grip and top speed
* Three lives, so a single bad corner doesn't instantly end the run
* A top five high score table saved in your browser
* Roadside life: pines, oaks, bushes, marker posts, road signs, billboards, windmills, a start gantry and clouds that drift past
* Traffic that mixes slow cruisers with low, striped speedsters that weave between lanes
* Keyboard controls on desktop, touch pads on phones

## Play it

```bash
git clone https://github.com/arpit-verma79/Ridge-Sprint-Car-Game.git
cd Ridge-Sprint-Car-Game
open ridge-sprint.html
```

On Linux use `xdg-open ridge-sprint.html`, on Windows use `start ridge-sprint.html`, or just drag the file onto a browser window.

If you'd rather serve it:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000/ridge-sprint.html

It's a static file, so GitHub Pages, Netlify or Vercel will host it as is.

## Controls

| Action | Keyboard | Touch |
| --- | --- | --- |
| Accelerate | Up arrow or W | GAS pad |
| Brake | Down arrow or S | BRAKE pad |
| Steer | Left and right arrows, or A and D | Arrow pads |
| Start or restart | Space or R | Start race button |
| Pick difficulty in the menu | 1, 2, 3 | Difficulty buttons |

## The cars

| Car | Style | Accel | Top speed | Grip |
| --- | --- | --- | --- | --- |
| Comet GT | Open wheel formula | 1.00 | 1.00 | 1.00 |
| Bolt X | Low wedge | 0.90 | 1.12 | 0.90 |
| Boulder 4x4 | Truck | 0.85 | 0.92 | 1.22 |
| Vesper R | Coupe | 1.18 | 0.96 | 1.06 |

These multipliers stack with the difficulty settings, so Bolt X on Pro is the fastest and by far the twitchiest thing in the game. Boulder 4x4 is the forgiving pick if the curves keep catching you out.

## Difficulty

| Level | Timer | Traffic | Fast cars | Grip | Top speed |
| --- | --- | --- | --- | --- | --- |
| Rookie | 75s | 26 | 10% | 1.15x | 0.92x |
| Normal | 60s | 44 | 28% | 1.00x | 1.00x |
| Pro | 50s | 66 | 48% | 0.86x | 1.08x |

## How scoring works

Your score is the distance you cover before the clock hits zero. Every crash costs a life and a big chunk of your speed, so smooth lines beat holding the throttle down and hoping. Run out of lives and the race ends early with whatever distance you'd managed.

After a hit you get about two seconds where the car blinks and can't lose another life, which keeps a single pile up from wiping all three at once.

Progress is stored in your browser under these keys:

* `ridgeSprintScores` for the top five table
* `ridgeSprintBest_easy`, `ridgeSprintBest_normal`, `ridgeSprintBest_pro` for the best run on each level
* `ridgeSprintCar` and `ridgeSprintDiff` for your last selections

Clear them from devtools if you want a fresh start.

## Poking at the code

Everything lives in `ridge-sprint.html`: markup, styles and game logic in one place.

The parts worth knowing about:

* Track building: `buildTrack`, `addRoad`, `addSegment`, `addScenery`
* Traffic and collisions: `buildTraffic` and the collision block inside `update`
* Projection and drawing: `project`, `render`, `renderSegment`, `drawBackground`, `drawSprite`, `drawCar`, `drawPlayer`
* Game state and UI: `startRace`, `finish`, `setCar`, `setDifficulty`, `updateHud`, `renderScores`

A few things I did to keep the frame rate steady:

* Traffic is bucketed by track segment once per frame instead of being scanned again for every drawn segment
* Sprites and cars that are off screen, sub pixel or below the viewport get skipped before any drawing happens
* The sky gradient is built once at startup rather than every frame
* HUD text is only written when the number actually changes

If it stutters on an older device, drop `drawDistance` or the per level `traffic` counts near the top of the script. Both are single numbers and take effect on the next race.

## Browser support

Anything current: Chrome, Firefox, Safari, Edge. It needs canvas, `requestAnimationFrame` and localStorage, all of which have been around for years. Touch pads appear automatically on screens narrower than 640px.

## License

MIT. Take it, fork it, add a lap counter.
