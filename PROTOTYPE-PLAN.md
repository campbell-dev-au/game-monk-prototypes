# Prototype Plan

We are not building the game yet. We are building a series of **small, disposable prototypes**, each designed to answer one question, so we can decide the game's direction with evidence.

## Rules for every prototype

- Each prototype has a **question**, a **timebox**, **success criteria**, and a **what we do if it fails** note.
- Grey-box only: cubes, capsules, flat colours. No art, no polish beyond hit feedback.
- Prototype code is allowed to be rough, but keep tuning values in Resources and keep the code readable. Anything that proves out gets cleaned up when it graduates to the real project.
- At the end of each prototype, write a short entry in `PROTOTYPE_LOG.md`: what we built, what we learned, what we decided, what changed in `GAME_DESIGN.md`.
- Do not start the next prototype until the previous one has a logged verdict (**keep / change / drop**).
- Playtesting is done by the human. Claude Code cannot judge feel, so it should ask for feedback and propose tuning knobs rather than claim something "feels good".

## Prototype 0: Skeleton (timebox: 1-2 sessions)

**Question:** Can we get a clean Godot 4 C# project running with the fixed isometric camera and a controllable monk?

Scope:
- Godot 4 .NET project, builds with `dotnet build`
- Orthographic camera at a fixed isometric angle, flat ground, a few cube obstacles
- Monk (capsule) moves with keyboard/gamepad, camera follows
- Project structure and conventions from `CLAUDE.md` in place
- A headless run path so tests and sims can execute without the editor UI

Success: project builds cleanly, monk moves, camera looks right, structure is agreed.

## Prototype 1: Duel feel (timebox: 3-5 sessions)

**Question:** Is fighting a single raider fun with just cubes?

Scope:
- Monk: light attack, heavy attack, dodge (stamina), parry with a visible timing window
- One Spearman: telegraphed attack, simple state machine (approach, wind-up, attack, recover, stagger)
- Posture/stagger meter on the raider
- Hit feedback: hit-stop, knockback, flashes, placeholder sounds
- Tuning values (timings, damage, stamina, posture) in Resources, ideally adjustable at runtime via a debug panel

Success: a 30-second duel is engaging, a skilled player visibly beats an unskilled one, and parrying feels risky and rewarding rather than a free win.

If it fails: the system lacks depth or feel. Change the move set or the timing model before adding any enemies.

## Prototype 2: Second enemy (timebox: 2-3 sessions)

**Question:** Does a second enemy type require a genuinely different answer?

Scope:
- Shield bearer: blocks frontal hits, vulnerable to flanking, guard-break, or shoving
- Both enemy types in the same arena, 1-3 at a time

Success: players must change tactics per enemy type, and mixed groups create interesting decisions.

If it fails: enemy variety is cosmetic. Rework the roster around mechanics, not stats.

## Prototype 3: First night mini-raid (timebox: 4-6 sessions)

**Question:** Is the first night, as pitched, fun, tense, and replayable?

Scope:
- Small monastery map: gate, courtyard, a few buildings, a beach with a longship
- 4-6 raiders using the raid definition resource (types, spawn times, spawn points)
- Raiders go for loot items and flee to the longship; a few fight the monk
- 2-3 improvised interactions (bar a door, tip a cart, ring the bell, light a brazier)
- Night timer, end-of-night summary (loot lost/saved, damage, flawless bonus)
- Event bus carrying the events the summary needs

Success: within three minutes the player feels pressure, makes meaningful choices (what to protect, where to stand), and wants to run it again. Different runs play out differently.

If it fails: add decisions (not difficulty). Revisit raider goals, loot placement, and the improvised tools.

Also decide here: survivable-but-painful first night vs scripted loss.

## Prototype 4: Skill vs investment simulation (timebox: 3-4 sessions)

**Question:** Can a skilled player realistically survive the first X nights with no defences, and do defences still matter?

Scope:
- Headless simulation harness running thousands of raids
- Bot "players" with tunable competence (reaction delay, parry success rate, positioning quality)
- Placeholder defences (e.g. a wall, a trap, an archer post) with simple stats
- Raid difficulty curve defined as data
- Output: survival rate and loot-loss by night, per bot skill level and per defence investment level

Success: a high-skill, zero-investment bot survives several nights with acceptable losses, a low-skill bot needs defences by about night 2-3, and defences visibly reduce risk without making the monk irrelevant.

If it fails: adjust the curve, damage model, or defence strength. The sim tells us when the skill ceiling is accidentally a wall or defences trivialise combat. Final judgement is still a human playtest.

## Prototype 5: Day loop stub (timebox: 3-5 sessions)

**Question:** Does the day phase create meaningful, satisfying choices that feed the night?

Scope:
- Day/night cycle with a day timer
- One passive economy building (lumber yard) that the monk builds and upgrades; it produces on its own
- Resource spend: build or upgrade a defence, or bank for later
- Basic gathering to bootstrap the first resources
- State persists from day to night and back, including damage and losses

Success: spending decisions feel meaningful and the "bank vs build" tension exists. The loop day, night, summary, day makes the player want one more day.

If it fails: the day phase is a chore. Cut or compress it, or move more decisions into it.

## Prototype 6: Visual direction test (timebox: 2-3 sessions)

**Question:** Does the orthographic 3D isometric approach give readable combat and a look we're happy with?

Scope:
- Take the prototype 3 scene and test it with a couple of free asset packs (e.g. Kenney or similar) and a day/night lighting pass
- Test enemy telegraph readability, silhouette clarity, and camera occlusion (buildings hiding enemies)
- If time allows, a quick comparison with a 2D isometric approach for one enemy and one building

Success: telegraphs and threats are readable at game scale, the look is achievable solo, and the art pipeline is realistic.

If it fails: adjust camera angle, add occlusion handling (fade or cutaway), or reconsider the view.

## After the prototypes: decision gate

Review `PROTOTYPE_LOG.md` and `GAME_DESIGN.md` and answer:
1. Is the night combat fun enough to carry the game?
2. Does the skill-vs-investment trade-off work as intended?
3. Does the day loop add to the experience rather than interrupt it?
4. Is the scope (adventure + building + defence) achievable solo, or should it be trimmed?
5. Is there a clear, commercially plausible hook?

If yes: start the real project (reuse proven code), set up a Steam page, begin a vertical slice. If not: change direction on the specific weak point, or stop.

## Parking lot (ideas to revisit later, not now)

Town repopulation mechanics, additional raider types (berserker, archer), story and characters, seasons, multiple raiding factions, co-op, save system design, localisation, audio design.
