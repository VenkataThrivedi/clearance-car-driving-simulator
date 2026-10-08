# Clearance: Car Driving Simulator

A browser-based car driving simulator for learning steering geometry, wheel paths, parking clearance, and spatial awareness. Built with HTML, CSS, and vanilla JavaScript in a single self-contained HTML file.

Clearance is a visual driving geometry lab, not a racing game. Change the steering and immediately see how the front wheels, rear wheels, and vehicle body would travel through a turn.

**No dependencies. No build step. No backend. Works offline.**

> Educational simulator only. This simulation simplifies real vehicle dynamics and is not a substitute for supervised real-world driving instruction.

## Run Locally

1. Download or clone this repository.
2. Open [car-driving-margin-simulator.html](car-driving-margin-simulator.html) from your computer in a modern browser such as Chrome or Edge.
3. Adjust the steering slider to explore the projected paths, or use the driving controls to move.

No Node.js, npm, web server, or internet connection is required to run the downloaded application.

## Features

- **Four individual wheel paths:** front-left, front-right, rear-left, and rear-right trajectories derived from vehicle geometry.
- **Body corridor:** projected body occupancy, corner paths, and a future vehicle outline.
- **Steering geometry:** separate inner and outer front-wheel angles, turning center, radius, and diameter.
- **Rear wheel offtracking:** a visual measurement of how much tighter the inside rear wheel's circle is than the inside front wheel's circle.
- **Live clearance:** left, right, front, and rear body measurements, plus a configurable safety margin.
- **Collision feedback:** body and wheel contact checks, movement stopping, and projected contact warnings.
- **Interactive steering wheel:** drag to rotate, with a configurable steering ratio, sensitivity, and optional auto-centering.
- **Vehicle presets:** small car, sedan, and SUV, with editable dimensions and overhangs.
- **Scene editor:** add, drag, resize, rotate, and delete cones, walls, curbs, parked vehicles, and parking spaces.
- **Practice scenarios:** nine beginner exercises, a steering studio, and randomized margin training with scoring.
- **View controls:** zoom, pan, follow the car, toggle measurement layers, and inspect debug geometry.
- **Responsive layout:** side-by-side simulation and controls on desktop, stacked views on smaller screens.

## Controls

### Keyboard

| Key | Action |
| --- | --- |
| `W` / Up arrow | Accelerate forward; brake first if reversing |
| `S` / Down arrow | Brake while moving forward, then reverse |
| `A` / Left arrow | Steer left |
| `D` / Right arrow | Steer right |
| `Space` | Brake |
| `R` | Reset the vehicle to the scenario's starting position |
| `Delete` | Delete the selected scene object |

Click the simulation canvas before using driving keys. Keyboard driving shortcuts do not intercept typing in input fields.

### Mouse and Touch

- Adjust the steering slider or drag the virtual steering wheel.
- Hold the on-screen direction buttons to drive; use the brake button to stop.
- Use **D / R** to choose the projected direction while stationary.
- Drag the car or an obstacle to reposition it.
- Drag empty ground to pan; scroll over the canvas or use the zoom buttons to change scale.
- Select an object in **Scene editor** to edit its dimensions, position, or rotation.

**Reset vehicle** resets the car and training score. **Reset scene** also restores the current scenario's original objects. Reloading the page discards scene edits and settings; there is no save or export feature.

## Try a First Experiment

1. Open **Steering studio** and enable **Where will my wheels go?** to pause movement and inspect the projection.
2. Compare steering inputs of `0`, `-5`, `-10`, `-20`, and `-30` degrees. Negative input turns left; positive input turns right.
3. Watch the front-wheel angles and turning radius update together.
4. Compare the cyan front-wheel paths with the amber rear-wheel paths. Solid lines indicate left wheels; dashed lines indicate right wheels.
5. Check the shaded body corridor, not just the wheel tracks, for possible contact.
6. Add or move a cone near the inside of the turn and observe the clearance and projected contact warnings.

Driving input leaves path-study mode and resumes movement. Projection distance is adjustable from **1 to 20 meters**; entering path-study mode brings it into the **5 to 10 meter** range.

## Practice Exercises

| Exercise | Focus |
| --- | --- |
| Straight driving | Maintain balanced clearance between lane boundaries |
| Small steering | Compare how small steering changes affect the path |
| Full left turn | Inspect wheel paths and turning geometry at left lock |
| Full right turn | Compare the mirrored right-hand turn |
| Narrow lane | Keep the body and safety margin within the boundaries |
| The 90-degree corner | Account for the inside rear wheel and front overhang |
| Parallel parking | Reverse into a marked space between parked vehicles |
| Close to a wall | Practice with a 30 cm, 50 cm, or 1 m clearance target |
| Rear wheel tracking | Compare front and rear wheel paths around a cone |

### Margin Training

Generate a randomized course and pass four cone gates. Lane width accounts for the current vehicle width and safety margin when the course is generated.

| Event | Points |
| --- | ---: |
| Collision | -100 |
| Safety margin violation | -20 |
| Very close pass | -5 |
| Clean pass | +10 |
| Centered driving | +5 |

Contact and safety penalties are applied on entry rather than every animation frame. Editing the scene or manually repositioning the vehicle exits training. Use **New course** to generate another layout.

## How the Geometry Works

The simulator uses a low-speed kinematic bicycle model with Ackermann-style front-wheel steering. The vehicle reference point is the **rear axle midpoint**, not the center of the body.

For wheelbase $L$ and effective road-wheel steering angle $\delta$, the rear-axle turning radius magnitude is:

$$
R = \frac{L}{\tan(|\delta|)}
$$

At zero steering, movement and projections are straight lines. Otherwise, each wheel's trajectory is generated by rotating its actual position around the same instantaneous turning center. Inner and outer front wheels receive different steering angles so their rolling directions are tangent to their respective circles.

- Vehicle-local **X** points forward and **Y** points left.
- World **X** points right and **Y** points up; heading is counterclockwise from world X.
- The UI uses negative steering for left and positive for right; the model converts this to its mathematical sign convention.
- Steering-wheel rotation equals the effective road-wheel angle multiplied by the configured steering ratio. These are not the same angle.
- Projections assume the current steering stays constant and use the current travel direction, or the selected direction while stationary.
- The displayed radius and diameter describe the rear-axle midpoint's circle, not the outer body or curb-to-curb turning circle.

### Understanding Clearance

Directional body readings measure the nearest gap along each side's outward direction. **Open** means no obstacle lies in that side's measurement corridor; it does not mean the entire surrounding area is empty.

Safety checks separately use the shortest polygon-to-polygon distance, including the wheels and diagonally positioned obstacles. An object can therefore trigger a safety warning without appearing in a directional body-clearance reading. Parking-space markings are not collidable obstacles.

## Publish with GitHub Pages

1. Push this repository to GitHub, keeping the HTML file in the repository root.
2. Open **Settings > Pages**.
3. Under **Build and deployment**, select **Deploy from a branch**.
4. Select the branch containing the application and the **/ (root)** folder, then save.
5. After deployment completes, open your Pages site with the application's full filename in the URL.

For a repository named `clearance-car-driving-simulator`, the application URL follows this pattern. Replace `YOUR_USERNAME` with your GitHub username or organization:

```text
https://YOUR_USERNAME.github.io/clearance-car-driving-simulator/car-driving-margin-simulator.html
```

Use that full URL when sharing the simulator. Opening only the repository's Pages root is not the same as opening the application file.

## Development and Limitations

All application code, styles, and graphics live in [car-driving-margin-simulator.html](car-driving-margin-simulator.html). `Geometry` contains the coordinate transforms, steering model, projections, and polygon calculations. `Simulator` manages driving state, rendering, controls, scenarios, and training.

Edit the HTML file and reload it in the browser. **Vehicle & view > Developer / geometry debug** exposes coordinates, heading, track width, wheelbase, steering angles, and the instantaneous turning center.

The model does not simulate tire slip, suspension, road gradients, realistic braking distances, or steering forces. Collision checks use simplified polygons and sampled movement. Treat clearances and projected paths as learning aids, not measurements suitable for operating a real vehicle.

## Contributing

Bug reports and focused improvements are welcome. For geometry issues, include the exercise, vehicle dimensions, steering input, direction of travel, browser, and steps to reproduce. A screenshot with geometry debug enabled can help explain the problem.

Please keep the application self-contained and dependency-free.
