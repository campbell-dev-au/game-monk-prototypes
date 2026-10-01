# Monastery Defence Game: Design Notes (working title: TBD)

Status: pre-prototype. Everything here is a hypothesis to test, not a commitment. Update this file when prototypes change our minds.

## Pitch

The player is a monk who wakes to find the monastery deserted: it is about to be raided (Vikings, in the Lindisfarne tradition). He stays and defends it. By day he gathers supplies and builds, repairs and upgrades defences and economic buildings. By night he fights off raids. Over time the monastery becomes a citadel, and the neighbouring town, also evacuated at the start, repopulates as the monk's activity grows the local economy.

Inspirations: Stardew Valley (daily rhythm, cosy progression, town revival), tower defence, Kingdom Two Crowns, Dome Keeper, Graveyard Keeper.

## Pillars

1. **Skill can substitute for investment.** A min-maxing player should be able to say "I can fight off the first X nights with no defences and bank the money for growth." Combat must have a real skill ceiling that does not depend on purchases.
2. **Raiders want things, not just walls.** Raiders go for loot (relics, silver, books, food) and flee to their longship. This creates decisions: what to protect, who to chase, where to stand.
3. **Graded failure.** A bad night costs goods and progress, not the whole run. Skipping defences is a gamble with a price.
4. **Passive growth.** Economy buildings (lumber yard, mine) are areas the monk builds and upgrades, which then produce on their own. Direct gathering is mainly an early-game bootstrap.
5. **Readable combat.** Clear telegraphs, strong silhouettes, a camera that never hides threats.

## Core loop

- **Day:** gather, build, repair, upgrade. Spend resources on defences or economy. Town and economy progress.
- **Night:** a raid arrives from the sea. Defend loot and the monastery using skill, improvised tools, and built defences. A summary screen shows what was lost, what was saved, and any bonuses.
- **Between:** the town repopulates and the economy grows, driven by performance and investment.

## The first night (the opening, and the first thing to prototype)

The player starts with no defences and no tutorial. A small band (about 4-6 raiders) arrives. The monk can fight, and can use improvised things already around the monastery (bar a door, tip a cart across a path, ring the bell, light a brazier). These preview later defence-building without explaining it.

Intended outcome: survivable but painful. The monastery is damaged and some loot is stolen, and that damage carries into day one. (The alternative, a scripted loss, is easier to balance but risks feeling unfair. Revisit after prototyping.)

## Combat design hypotheses

Monk toolkit (quarterstaff-flavoured):
- Light and heavy attacks with distinct tradeoffs (speed vs reach and stagger)
- Dodge with limited stamina
- Parry or perfect block with a tight timing window that staggers or punishes attackers
- Posture/stagger meter on enemies: deflecting and breaking guard is as effective as raw damage
- Positioning tools: knockback, shoving enemies into each other, fighting in doorways

Gear can widen timing windows, reduce stamina costs, and add damage, but the base skill is present from minute one.

### Raider roster (initial ideas)

| Type | Behaviour | Intended counter |
|---|---|---|
| Spearman (fodder) | Attacks in lines, long poke | Funnel into chokepoints, sidestep, parry |
| Shield bearer | Blocks frontal hits | Flank, heavy attack to break guard, shove |
| Berserker | Heavy, committed swings | Well-timed parry; punishes panicked dodging |
| Archer / thrower | Ranged pressure | Move, use cover, prioritise |
| Looter / runner | Grabs loot, flees to boat | Intercept, choose between chasing and holding |

Every enemy has a visible wind-up. Start with Spearman and Shield bearer only.

## Economy and difficulty hypotheses

- Raid difficulty scales on a fixed curve, independent of what the player has built.
- Defences let the player handle bigger raids with less risk. Skilled play handles them with more risk.
- A pure-skill run should be viable for the first several nights, then become steadily riskier.
- Flawless nights give bonuses (extra resources, faster town repopulation).
- Defences should handle crowds. The monk should stay relevant against elites, otherwise combat depth stops mattering later.

## Technical direction (initial, revisit after prototype 6)

- Engine: Godot 4 with C# (.NET build)
- View: fixed-angle orthographic camera for an isometric look, 3D low-poly or stylised assets (rationale: cheaper than true 2D isometric art, free lighting for day/night). Prototype 6 tests this.
- Data-driven: enemy stats, weapons, defences, and raid definitions live in Godot `Resource` files so tuning does not need code changes.
- Event bus for game events (loot stolen, raider killed, etc.) so the summary screen and later economy systems can hook in.
- Headless simulation tests for combat and balance.

## Out of scope (for now)

Art beyond placeholder assets, the town repopulation layer, the full economy, story and dialogue, audio beyond hit-feedback placeholders, Steam page and marketing. These come after the prototypes tell us the core is fun.

## Commercial notes

Realistic expectations: most Steam releases earn little. If the prototypes prove out, the likely path is an early Steam page, p[118;1:3uublic GIFs, a demo, and Steam Next Fest. Decide deliberately about AI-generated art, since Steam requires disclosure and some players react negatively.

## Open questions

- Is the monk primarily a fighter, a tactician, or both? (Prototypes 1-3 and 4 inform this.)
- 3D orthographic or true 2D isometric? (Prototype 6.)
- Survivable-but-painful first night, or scripted loss? (Prototype 3.)
- How many nights can a zero-investment run realistically survive? (Prototype 4.)
- Is there enough decision-making in a night to carry the game, or does it need more day-time depth?
