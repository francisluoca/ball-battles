# Orbit — Ball Battle

## Product goal

Create a browser arena game where the player upgrades an automatically moving ball, battles bots, and earns random upgrades, ball types, and skins by opening mystery boxes. Higher player levels bring tougher bots.

The current prototype lives in `game.html` and runs as a standalone page with no build step.

## Core gameplay loop

1. Start a run as the Classic ball with the Mint skin, 100 HP, and 200 credits for an initial box roll.
2. Watch the ball move automatically, bounce off arena walls, and attack with two orbiting knives; automatically shoot at the nearest bot only after unlocking Shooting. Movement cannot be controlled by the player.
3. Eliminate bots to earn XP and credits.
4. Spend credits to open boxes and receive random rewards.
5. Clear waves, level up, and face stronger bots until health reaches zero.
6. Review the final level, wave, and eliminations, then restart.

## Combat and waves

The starting weapon is a pair of orbiting knives. Knives remain active after Shooting is unlocked. Bots never shoot. They move automatically in random starting directions, bounce off all four arena borders, and attack with two orbiting knives, dealing `8 + level × 2` damage per hit with a 0.55-second per-bot cooldown.

- Use a compact 400 × 300 arena with a top-down view, visible projectiles, bot health bars, and hit particles.
- Bots also move continuously, reflect their direction at each wall, and remain fully inside the arena. Their movement speed increases with player level.
- The player ball continuously moves diagonally and reflects its direction on each wall impact, staying entirely inside the arena. Speed upgrades and ball types modify its automatic movement speed.
- Start at wave 1 with three bots. Each subsequent wave adds one bot, up to ten per wave.
- A wave ends after every bot is eliminated. Allow a short break before the next wave.
- Restore 20 HP when advancing to a new wave, capped at maximum health.
- End the run at zero HP.

## Progression and difficulty

- Start at level 1 with zero XP.
- XP required for the next level: `60 + (level - 1) × 35`.
- Each elimination awards `22 + level × 3` XP and `15 + level × 3` credits, using the level at the time of the award.
- Leveling up grants 30 credits and restores 25 HP, capped at maximum health.
- Higher levels increase bot health, movement speed, knife damage, knife rotation speed.
- Bot health is determined when a bot spawns; other difficulty values use the current player level.

| Player level | Bot tier |
| --- | --- |
| 1–2 | Rookie |
| 3–5 | Hunter |
| 6–9 | Elite |
| 10+ | Apex |

## Mystery boxes

Boxes are the way to obtain upgrades, ball types, and skins. There is no direct upgrade purchase menu.

- Each box costs 200 credits earned through gameplay.
- The starting 200 credits cover one initial roll.
- Disable opening when the player cannot afford a box, a run is inactive, or another box is opening.
- Deduct the cost once, freeze combat, and show a visible reward reel that cycles through upgrade, ball type, and skin names for about three seconds, slowing down before highlighting the final prize.
- Award exactly one reward per box and display its name and effect. Preview entries do not award items or change the category probabilities.
- Keep the claim button disabled while the reel is spinning. Respect reduced-motion preferences by disabling the reel tick animation.
- Keep combat frozen until the player selects **Back to battle**. Preserve an existing manual pause.
- New types and skins unlock in the loadout menu; the current selection remains equipped. Duplicate types or skins award 20 credits instead.
- Rewards, ownership, upgrades, XP, and credits reset when a new run starts. There is no saved progression in this version.

| Reward category | Chance | Selection |
| --- | --- | --- |
| Stat upgrade | 50% | Weighted rarity tier, then equal chance within that tier |
| Ball type | 25% | Equal chance among Titan, Comet, and Pulse |
| Skin | 25% | Equal chance among four unlockable skins |

### Upgrades

The box pool contains 30 upgrades across eight rarity tiers within the 50% upgrade category. Roll a rarity tier first, then select uniformly among upgrades in that tier. All stack; explicit caps are shown below. Shooting-related upgrades can be rolled before Shooting and become active when it is unlocked. The panel includes a full collection guide and a list of acquired ranks.

| Upgrade | Effect per rank |
| --- | --- |
| Damage | Add 7 knife and shot damage. |
| Attack speed | Increase knife rotation and firing speed by 20% of base. |
| Speed | Add 22 movement speed. |
| Max health | Add 30 max HP and restore 45 HP. |
| Shooting | Unlock automatic shots; later ranks add 5 shot damage. |
| Extra knives | Add another orbiting knife (maximum 14). |
| Knife reach | Extend knife orbit by 4 units (maximum +40). |
| Sharp blades | Add 6 knife damage. |
| Critical chance | Add 5% critical chance (maximum 75%). |
| Critical power | Add 25% critical damage. |
| Poison | Hits poison bots for 3 seconds: 3 damage/second per rank. |
| Fire blades | Hits burn bots for 2 seconds: 5 damage/second per rank. |
| Frost | Hits slow bots for 2 seconds, 8% per rank (maximum 60%). |
| Lifesteal | Heal for 2% of direct damage per rank (maximum 20%). |
| Regeneration | Recover 0.5 HP per second per rank. |
| Plating | Reduce incoming damage by 6% per rank (maximum 60%). |
| Dodge | Add 4% chance to evade knife hits (maximum 50%). |
| Shield | Gain 20 shield capacity per rank; refills each wave. |
| Thorns | Reflect 3 damage per rank on bot knife hits. |
| Damage aura | Deal 3 damage/second per rank to bots within 65 units. |
| Execution | Deal 15% more damage per rank to bots below 30% health. |
| Bounty | Earn 4 extra credits per kill per rank. |
| Wisdom | Earn 15% more XP per rank. |
| Salvage | Duplicate refunds increase by 15 credits per rank. |
| Wave medic | Heal 10 additional HP per rank after each wave. |
| Victory heal | Restore 2 HP per kill per rank. |
| Multishot | With Shooting: add one spread projectile (maximum 7 shots). |
| Heavy projectiles | With Shooting: increase projectile radius and add 4 shot damage per rank. |
| Piercing | With Shooting: pass through one extra bot per rank. |
| Explosive shots | With Shooting: impacts deal 5 splash damage per rank within 45 units. |

Direct knife and projectile hits apply criticals, lifesteal, poison, burn, and frost. Damage over time, aura, reflected damage, and splash do not recursively trigger these effects. Shields absorb damage after plating and refill each wave. All upgrades reset on a new run. Bot weapons remain knives only.

### Upgrade rarities

Tiers run from least to most rare in this order: Uncommon, Common, Rare, Ultra Rare, Epic, Legendary, Mythic, Secret. Each has a visible name and distinct color in the reel, reveal, guide, and equipment menu. The expandable Rarity odds panel shows tier probabilities; the guide and reward details show the specific upgrade's chance per box.

Boxes remain 50% upgrade, 25% ball type, 25% skin. Within upgrades, roll the tier first and select uniformly within that tier. Specific upgrade odds per box are `50% × tier probability ÷ upgrade count in tier`. Reel previews do not determine rewards. Ball types and skins retain their existing selection rules.

| Tier | Color | Upgrade-roll chance | Per-box tier chance | Upgrades |
| --- | --- | --- | --- | --- |
| Uncommon | #7ee39b | 40% | 20% | Damage, Speed, Max health, Knife reach, Sharp blades, Wave medic, Salvage, Heavy projectiles |
| Common | #b8c4d8 | 30% | 15% | Attack speed, Critical chance, Critical power, Thorns, Bounty, Wisdom |
| Rare | #72b6ff | 16% | 8% | Regeneration, Poison, Fire blades, Frost, Victory heal |
| Ultra Rare | #46e5ee | 8% | 4% | Plating, Dodge, Piercing, Shield |
| Epic | #c18cff | 4% | 2% | Shooting, Extra knives, Execution |
| Legendary | #ffd166 | 1.5% | 0.75% | Multishot, Lifesteal |
| Mythic | #ff7cad | 0.45% | 0.225% | Damage aura |
| Secret | #ff9466 | 0.05% | 0.025% | Explosive shots |

### Equipment selection

- The loadout menu offers dropdowns for owned ball types and skins and a checkbox for each owned upgrade.
- Only owned equipment is selectable. Choices apply immediately during a run, including while manually paused, and are locked during box reveals or after game over.
- New upgrades start equipped. Repeated rolls increase owned ranks. If an upgrade is unequipped, further rolls leave it unequipped until the player enables it.
- Equipping an upgrade activates all owned ranks; unequipping retains ownership but removes its ongoing effects. There is no limit on equipped upgrade types.
- Unequipping Max health clamps current HP to the reduced maximum. Re-equipping does not heal. Shield toggles do not refill shield; it refills on a new wave or increases from an equipped Shield reward.
- Unequipping Shooting clears existing player projectiles. Previously applied poison, burn, and frost timers expire normally; their damage and slow effects depend on currently equipped ranks.
- Rewards such as credits and prior healing are retained when the related upgrade is unequipped. Loadout and ownership reset on restart.

### Ball types

Only one ball type is active at a time. Use the Ball type dropdown to choose any owned type, including Classic. New unlocks do not replace the current selection.

| Type | Effect |
| --- | --- |
| Classic | Starting ball with balanced stats |
| Titan | 55% more damage; 22% slower movement |
| Comet | 30% faster movement; 15% less damage |
| Pulse | 50% faster firing; 20% less damage |

### Skins

Skins change the player's ball color without affecting combat stats. Mint is the starting skin. Boxes can award Solar Gold, Nebula Violet, Arctic Blue, or Rose Quartz. Use the Skin dropdown to choose any owned skin, including Mint. New unlocks do not replace the current selection.

## Controls

| Input | Action |
| --- | --- |
| Automatic | Move continuously and bounce off walls |
| Automatic | Fight with two orbiting knives; shoot at the nearest bot only with the Shooting upgrade |
| Open box / Back to battle | Roll and claim rewards |
| P or Pause button | Pause or resume |

Keyboard, mouse, and touch inputs do not steer the ball or control its attacks. Losing window focus pauses combat. Pausing or opening a box freezes movement and combat.

## Interface

- Show current HP, maximum HP, wave, and elimination count above the arena.
- Show player level, XP progress, credits, bot tier, active type, skin, and upgrade ranks in the side panel.
- Show box price, category probabilities, duplicate compensation, and the run-only reward policy.
- Provide start, pause/resume, box-opening, reward-claim, and restart controls.
- Use a dark arena with a grid, bright balls, and contrasting enemy knives.
- Fit the game to the viewport height with the panel beside the arena on desktop and below it on narrow screens. Keep the arena visible; scroll long equipment and upgrade lists inside the panel.
- Preserve the arena aspect ratio without stretching or cropping. Constrain box dialogs to the viewport and allow internal scrolling. Compact the title screen on short displays.

## Acceptance criteria

- A new run begins with the documented starting state and enough credits for one box.
- The player ball moves without input, bounces off all four walls, and stays within arena bounds.
- Two visible knives orbit the ball and damage bots on contact, with a 0.3-second per-bot hit cooldown.
- Bots use knives only and never fire projectiles.
- The player fires no projectiles until Shooting is rolled from a box. Once unlocked, shots automatically target the nearest bot.
- Keyboard, mouse, and touch cannot steer the ball.
- Hits, eliminations, and game over work during automatic battles.
- Clearing a wave advances the wave and heals the player without exceeding maximum HP.
- Earning enough XP increases the player level, grants the level reward, and raises bot difficulty.
- A box deducts exactly 200 credits and awards one reward according to the documented probabilities.
- Stat upgrades stack, ball types affect gameplay, and skins affect appearance only.
- Duplicate types or skins grant exactly 20 credits.
- No combat or damage occurs while paused or during a box reveal.
- Restarting resets the run, including all rewards and ownership.

## Current validation status

HTML parsing and referenced element IDs have been checked. Browser gameplay, touch controls, reward outcomes, and difficulty balance still require interactive testing.
