# CLAUDE.md

## What this project is

A prototype series for a game: a monk defends a monastery from nightly Viking raids and, over time, builds it into a citadel that repopulates the neighbouring town. See `GAME_DESIGN.md` for the design and `PROTOTYPE_PLAN.md` for the plan. We are **prototyping to find direction**, not building the final game.

Read `GAME_DESIGN.md` and `PROTOTYPE_PLAN.md` at the start of each session. The current prototype and its status are tracked in `PROTOTYPE_LOG.md` (create it if it does not exist).

## Working style

- Work in **small steps**. Implement one mechanic at a time and stop so the human can playtest. Do not build several systems in one go.
- You cannot judge game feel. Do not claim something "feels good". Instead, expose tuning knobs, describe what changed, and ask for feedback.
- Prototype code can be rough, but keep it readable and in the structure below. If something is a hack, mark it with `// PROTOTYPE:` so it can be cleaned up when it graduates.
- Ask before adding Godot addons, NuGet packages, or any dependency.
- Do not add features outside the current prototype's scope. Put ideas in the parking lot in `PROTOTYPE_PLAN.md` instead.
- Keep design decisions in the docs. When a decision changes, update `GAME_DESIGN.md` and log it in `PROTOTYPE_LOG.md`.
- The human wants to stay close to the code, so explain non-obvious architectural choices briefly and prefer simple, conventional C# over clever abstractions.

## Tech stack

- Godot 4.x, .NET (C#) edition. Verify the installed Godot version and the .NET SDK version before scaffolding, and record both in this file.
- Fixed-angle orthographic camera for an isometric look, 3D grey-box assets.
- Everything is placeholder art: primitives (cubes, capsules, planes) and flat colours.

## Architecture conventions

- **Data-driven:** stats and definitions (enemies, weapons, defences, raid definitions, difficulty curve) live in Godot `Resource` files (C# classes with `[GlobalClass]` where useful), not hardcoded. All combat timings (wind-ups, parry window, stamina costs, posture values) are tunable data.
- **State machines:** characters and enemies use explicit, readable state machines (plain C# classes or enums, no heavy framework).
- **Event bus:** gameplay events (loot stolen, raider killed, parry succeeded, night ended) go through a central event bus, so the summary screen, later economy systems, and the simulation harness can subscribe without tight coupling.
- **Separate logic from nodes where practical:** keep combat and balance rules in plain C# classes that can run without the scene tree, so they can be tested and simulated headlessly. Godot nodes handle presentation and input.
- **Debug tooling is welcome:** a runtime tuning panel, on-screen hitbox and state debug, and slow-motion toggles are worth building early because they speed up iteration.

## Suggested folder layout

```
/docs                 (design docs, prototype log)
/src
  /Core               (event bus, shared utilities, state machine base)
  /Combat             (monk, enemies, damage, posture, parry logic)
  /Data               (Resource class definitions)
  /Game               (night/day flow, raid director, summary)
  /UI                 (debug panel, HUD, summary screen)
/scenes
/resources            (.tres data files)
/tests                (headless tests and simulation harness)
```

Adjust if Godot conventions suggest otherwise, but keep the logic/presentation split.

## Build, run, and test

- Build with `dotnet build` and fix all compiler warnings you introduce.
- Run the game headless where possible for tests and simulations (Godot's headless mode). Record the exact commands that work in this section once confirmed.
- Unit-test pure C# combat and balance logic. For prototype 4 onwards, maintain a simulation harness that runs many raids with bot players of configurable skill and prints survival and loot-loss statistics.
- Before saying a change works, build it and run the relevant tests or the headless run, and report the result.

## Definition of done for a prototype step

1. It builds and runs.
2. The new mechanic is playable by the human, with tuning knobs exposed.
3. `PROTOTYPE_LOG.md` has a short note of what was built and what to look for when playtesting.
4. Open questions or surprises are written down, not silently worked around.

## Things to avoid

- Building art pipelines, menus, save systems, or content beyond the current prototype.
- Premature optimisation and over-engineered frameworks.
- Large refactors during a prototype unless the human asks.
- Changing design intent without flagging it. If implementation reveals a design problem, stop and raise it.
