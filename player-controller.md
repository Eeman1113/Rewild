# Player Controller

> **Status**: Designed (pending review)
> **Author**: User + Claude Code agents
> **Last Updated**: 2026-04-17
> **Implements Pillar**: Pillar 2 — Mastery Through Understanding
> **Creative Director Review (CD-GDD-ALIGN)**: CONCERNS (accepted) 2026-04-17

## Overview

The Player Controller governs all aspects of the player creature's physical presence and movement in the world. It translates input into context-sensitive locomotion — running, jumping, climbing, crawling, swimming — across a 2D platformer space built on Godot's CharacterBody2D. The controller owns the creature's physics body, collision shape, movement state machine, and the feel layer that shifts movement characteristics based on environmental context (tense near predators, precise during traversal challenges, calm during exploration). It does not own combat, evolution effects, or visual presentation — those systems read the controller's state and modify behavior or appearance accordingly.

Without the Player Controller, the game has no 30-second loop. Every moment-to-moment interaction — dodging a predator's path, scaling a vine-wrapped overpass, squeezing through a collapsed tunnel, or simply walking through rain — flows through this system. It is both the most-touched system (7 dependents) and the system whose feel most directly determines whether the game is compelling or not.

## Player Fantasy

**You are always somewhere you are not supposed to be.**

Every space in the world belongs to something else — a predator's patrol route, a colony's nesting ground, a weather system's flood path. The player creature is not an intruder by choice but by existence. The movement fantasy is the fantasy of the trespasser: quick, quiet, improvised, always aware that the space you're moving through was never built for you and tolerates your presence only because it hasn't noticed you yet.

This is not a power fantasy. The creature never becomes the apex predator, never dominates a territory, never feels safe in the open. The satisfaction comes from a different place entirely — the specific pride of having passed through a space that should have killed you, the thrill of threading through the negative space in someone else's world.

**The moment-to-moment feeling shifts with context:**
- **Near predators**: Trespass as survival — desperate bursts, committed routes, no hesitation because hesitation is death
- **During traversal challenges**: Trespass as precision — testing handholds, reading surfaces, pressing through gaps the creature barely fits through
- **During exploration**: Trespass as observation — moving carefully through spaces that tolerate you, studying the world through the creature's body
- **In shelters**: The exhale — the only spaces where the creature belongs, where the trespass stops and the player can breathe

**The long arc is trespass becoming fluency.** Early game: every space is hostile and unfamiliar. Late game: the player knows patrol routes, weather cycles, material properties, gaps in the ecology. The creature still owns nothing, but it moves through the world like water through cracks — not because the world got easier, but because the player learned to read it. Fluency is mastery made physical. The same controls, the same creature, but a completely different movement vocabulary born from understanding.

**Pillar alignment:**
- **Pillar 1** (The World Doesn't Care): The creature is mechanically trespassing. Collision shapes, sight lines, and patrol routes exist for the organisms that own them, not as obstacles for the player.
- **Pillar 2** (Mastery Through Understanding): The trespasser must understand the space better than its owner. Knowledge is the lockpick — the player who knows the predator's timer and the weather's scatter pattern can thread through spaces that seem impassable.
- **Pillar 4** (You Become What You Survive): Evolution makes the creature a better trespasser in specific domains. Spore membranes let you trespass through fungal toxicity. Thermal resistance opens server farm heat zones. Each mutation is a skeleton key, but only for specific locks.

**Reference feel**: Closer to Rain World's slugcat than Celeste's Madeline — the creature has mass, momentum, and physical commitment. You pilot a body, not an avatar. But with tighter recovery than slugcat — the creature stumbles but catches itself. The skill ceiling is in reading the terrain well enough to never ask the body to do something it cannot do smoothly.

## Detailed Design

### Core Rules

#### CR-1. Physics Foundation

The Player Controller is a Godot CharacterBody2D. All movement is computed in `_physics_process(delta)` at 60fps (16.67ms per frame). Velocity is stored as a `Vector2` in pixels per second. Gravity, acceleration, and friction are applied every physics frame before calling `move_and_slide()`.

**Coordinate system**: +X is right, +Y is down. Gravity pulls in +Y. All speed values in this document are in pixels per second (px/s) unless stated otherwise. At 480x270 viewport resolution, the full screen width is traversed in 480 / MAX_RUN_SPEED seconds.

**Collision shape**: The base creature uses a capsule collider 10px wide x 12px tall (smaller than the 16x16 sprite to allow visual overlap with surfaces -- the creature looks like it is pressing against walls and floors, not floating above them). When crawling, the collider reshapes to 12px wide x 6px tall. Evolution may expand the collider up to 18px wide x 20px tall at 32x32 sprite stage. The collider dimensions are stored in the Evolution Interface (CR-9) and re-read on mutation.

**One-way platforms**: Tiles flagged as one-way (vines, thin ledges, fungal shelves) allow upward passage. The player drops through them by pressing Down + Jump simultaneously. Drop-through disables collision with the platform for 6 frames (100ms), then re-enables.

---

#### CR-2. Ground Movement (Walk/Run)

**Initiation**: Ground movement begins when the horizontal move input axis is non-zero while the creature is on the floor (`is_on_floor() == true`) and the current state is Idle, Walking, or Running.

**Walk/Run distinction**: There is no separate walk button. The creature walks at low input magnitude (analog stick < 0.4 or keyboard tap-and-release within 100ms) and runs at full input magnitude (analog stick >= 0.4 or keyboard hold). This is a continuous blend, not a binary toggle.

**Physics behavior**:
- Base walk speed: 60 px/s
- Base run speed: 120 px/s
- Ground acceleration: 600 px/s^2 (reaches run speed from standstill in 0.2s)
- Ground deceleration (input released): 900 px/s^2 (stops from run speed in 0.133s)
- Ground deceleration (input reversed): 1200 px/s^2 (turnaround from full run in 0.1s)
- Turnaround behavior: When input reverses while moving at >50% max speed, the creature enters a 4-frame (67ms) skid animation during which velocity decelerates at the reversal rate. The skid is visual commitment -- the creature's body leads the turn. This is where the "mass" feeling lives.

**Surfaces and context**: Ground movement acceleration and deceleration are modified by surface friction multipliers (see CR-7). Wet metal has 0.5x deceleration (slides further). Moss has 1.3x deceleration (stops faster). The base values above assume the default surface (dry stone, friction multiplier 1.0).

**Feel layer interaction**: The feel layer (CR-6) scales max speed and acceleration. In CALM context, values are as stated. In TENSE context, max run speed increases to 140 px/s and acceleration to 750 px/s^2 (adrenaline burst). In PRECISE context, max run speed drops to 90 px/s and deceleration increases to 1100 px/s^2 (tighter control).

**Input mapping**: Horizontal axis (left/right). Analog magnitude controls walk/run blend.

---

#### CR-3. Jumping

**Ground jump initiation**: Jump input while `is_on_floor() == true` or within the coyote time window (CR-3a). The creature leaves the ground with an initial vertical velocity applied as an impulse.

**Physics behavior**:
- Jump impulse: -280 px/s (upward; negative Y)
- Gravity while ascending: 800 px/s^2
- Gravity while descending: 1000 px/s^2 (heavier fall for commitment feel)
- Maximum fall speed (terminal velocity): 350 px/s
- Jump height from impulse: approximately 49px (3 tiles at 16px/tile)
- Time to apex: 0.35s
- Variable jump height: If the player releases the jump button before the apex, remaining upward velocity is multiplied by 0.4 immediately. This gives a minimum jump height of approximately 20px (1.25 tiles) when the button is tapped, and the full 49px when held.

**Air control**: While airborne, horizontal acceleration is reduced to 70% of ground acceleration (420 px/s^2). Horizontal deceleration in air is 30% of ground deceleration (270 px/s^2). The creature has meaningful air control but cannot reverse direction as sharply as on the ground. This is the "commitment" -- once you jump in a direction, changing course costs time.

**CR-3a. Coyote time**: After walking off a ledge (not jumping), the player has 6 frames (100ms) to input jump and still get a ground jump. Coyote time does NOT activate after wall-release or after being pushed off a ledge by an external force.

**CR-3b. Jump buffering**: If the player presses jump within 6 frames (100ms) before landing on a surface, the jump executes on the frame of landing. The buffer is consumed on use. Buffer applies to ground surfaces and wall surfaces (triggers wall jump if applicable).

**CR-3c. Wall jump**: See CR-4.

**Feel layer interaction**: In TENSE context, jump impulse increases to -300 px/s (higher jump, more desperate). In PRECISE context, gravity while ascending decreases to 720 px/s^2 (slower apex, more time to aim), and air control increases to 85% of ground acceleration.

**Input mapping**: Jump button (press to initiate, hold for full height, release for short hop).

---

#### CR-4. Wall Interaction (Wall Slide, Wall Cling, Wall Jump)

Wall interaction requires the creature to be airborne and pressing into a wall surface flagged as wall-interactive (most natural surfaces -- vine-covered walls, rough stone, bark -- are wall-interactive; smooth metal, wet glass, and ice are NOT unless evolution provides gecko pads or equivalent).

**CR-4a. Wall Slide**:
- **Initiation**: Creature is airborne, moving horizontally into a wall-interactive surface, and holding the directional input toward the wall.
- **Physics**: Fall speed is reduced to 80 px/s (from terminal velocity of 350 px/s). Gravity during wall slide is 200 px/s^2 capped at the 80 px/s slide speed. The creature descends the wall slowly and controllably.
- **Termination**: Releasing horizontal input away from the wall, pressing jump (triggers wall jump), reaching the floor (transitions to Idle/Walk), or the wall surface ending (transitions to Falling).
- **Duration limit**: The creature can wall-slide on the same wall for a maximum of 2.0 seconds before grip degrades. After 2.0s, slide speed increases by 50 px/s per second until the creature either jumps, reaches a floor, or hits terminal velocity. This prevents infinite wall-hugging.

**CR-4b. Wall Cling**:
- **Initiation**: During a wall slide, the player presses and holds the grab input (Up + toward-wall simultaneously, or a dedicated grab button if the input system provides one).
- **Physics**: Vertical velocity drops to 0. The creature holds position on the wall. Stamina is not tracked -- cling duration is unlimited but the creature is fully exposed (no movement, visible to predators). This is a deliberate risk-reward: cling to observe, but you are a stationary target.
- **Termination**: Releasing grab input (transitions to Wall Slide), pressing jump (triggers wall jump), pressing Down (drops to Falling with no coyote time).
- **Surface requirement**: Only surfaces with a wall_cling_allowed flag (rough stone, dense vine cover, bark). Smooth surfaces allow wall slide but not wall cling.

**CR-4c. Wall Jump**:
- **Initiation**: Jump input while in Wall Slide or Wall Cling state.
- **Physics**: Jump impulse of -260 px/s (vertical, slightly less than ground jump) combined with a horizontal impulse of 160 px/s away from the wall. For 8 frames (133ms) after a wall jump, horizontal input toward the wall is suppressed (the creature is committed to the jump arc). After 8 frames, full air control resumes.
- **Wall jump fatigue**: Consecutive wall jumps on the same wall (without touching the ground or a different wall) reduce vertical impulse by 20% per jump. First wall jump: -260 px/s. Second: -208 px/s. Third: -166 px/s. Fourth and beyond: -133 px/s (floor). This prevents infinite wall-jump climbing on a single wall. Touching the ground or a different wall resets the fatigue counter.
- **Feel layer interaction**: In TENSE context, the 8-frame input lockout is reduced to 5 frames (faster redirection possible under pressure). In PRECISE context, the horizontal impulse is reduced to 120 px/s (tighter, more vertical wall jumps for precision climbing).

**Input mapping**: Horizontal axis (toward wall to initiate slide), Grab input (cling), Jump button (wall jump).

---

#### CR-5. Climbing (Vine/Surface Climbing)

Climbing is a distinct locomotion mode for surfaces flagged as climbable (dense vine networks, root structures, fungal ladders, rope remnants). Climbing surfaces are distinct from wall-interactive surfaces -- a rough stone wall allows wall-slide but not free climbing. A vine-covered wall allows both.

**Initiation**: The creature contacts a climbable surface AND the player presses the grab input. Climbing does not auto-engage on contact -- the player must opt in. This prevents accidental climbing when the player intends to wall-slide past a vine patch.

**Physics behavior**:
- Climb speed (all directions): 50 px/s
- Climb acceleration: 400 px/s^2
- Climb deceleration: 800 px/s^2 (stops quickly on vines -- high grip)
- Gravity while climbing: 0 (fully supported by the surface)
- The creature can move in all four cardinal directions plus diagonals while climbing. Diagonal speed is normalized (50 px/s magnitude, not 70.7 px/s).

**Termination**:
1. Releasing the grab input: creature detaches and enters Falling state with 0 initial velocity.
2. Pressing jump: creature launches from the surface with an impulse of -240 px/s (vertical) and 100 px/s (away from surface). This is a climb-launch, not a wall jump -- it has no fatigue counter.
3. Reaching the edge of the climbable surface: if the surface ends at the top, the creature auto-mantles onto the ledge above (4-frame animation, 67ms, no input required). If the surface ends at the bottom or sides, the creature transitions to Wall Slide (if the underlying wall is wall-interactive) or Falling (if not).

**Surface types**: Different climbable surfaces have different climb speed multipliers:
- Dry vine: 1.0x (baseline)
- Wet vine: 0.7x (slippery, reduced grip)
- Root structure: 1.1x (rigid, good handholds)
- Fungal ladder: 0.8x (soft, compresses under weight)
- Rope/cable remnant: 1.0x speed, but sways -- creature position oscillates +/- 2px horizontally on a 1.5s sine cycle. Cosmetic but visually communicates instability.

**Feel layer interaction**: In TENSE context, climb speed increases to 65 px/s (frantic scrambling). In PRECISE context, climb speed drops to 40 px/s and deceleration increases to 1000 px/s^2 (very deliberate placement). In CALM context, values are as stated.

**Input mapping**: Grab input (to engage/disengage climbing), directional axes (to move on surface), Jump (to launch from surface).

---

#### CR-6. The Feel Layer

The feel layer is a parameter modifier system that sits between the raw movement constants and the physics simulation. It reads a `threat_context` enum from the World/Biome system and applies multipliers to a defined set of movement parameters. The feel layer does NOT change the state machine logic, the available transitions, or the input mappings. It changes only the numeric feel of existing behaviors.

**Threat contexts** (mutually exclusive, one active at a time):

| Context | Trigger Condition | Emotional Target |
|---|---|---|
| CALM | No predators within awareness radius (default 160px). No environmental hazards active. Player is in explored territory. | Solitary absorption. Movement is measured, unhurried. |
| TENSE | Predator detected within awareness radius but not in active pursuit. Environmental hazard nearby but not yet damaging. Uncharted territory. | Involuntary vigilance. Movement is sharp, ready, slightly faster. |
| PANIC | Predator in active pursuit (Creature AI has flagged pursuit state targeting the player). | Committed flight. Maximum speed, reduced control precision. |
| PRECISE | Player is within a traversal challenge zone (tight gaps, vertical climbs, moving platforms). Defined by level geometry flags. | Careful attention. Movement is slower, tighter, more controllable. |
| SHELTERED | Player is inside a shelter zone. | Exhale. Movement is slow, soft, no urgency. |

**Parameter modification table** (multipliers applied to base values):

| Parameter | CALM | TENSE | PANIC | PRECISE | SHELTERED |
|---|---|---|---|---|---|
| Max run speed | 1.0 | 1.17 | 1.33 | 0.75 | 0.5 |
| Ground acceleration | 1.0 | 1.25 | 1.5 | 0.9 | 0.6 |
| Ground deceleration | 1.0 | 0.9 | 0.75 | 1.22 | 1.3 |
| Jump impulse (magnitude) | 1.0 | 1.07 | 1.15 | 1.0 | 0.85 |
| Ascending gravity | 1.0 | 1.0 | 1.1 | 0.9 | 1.0 |
| Air control (% of ground accel) | 0.7 | 0.75 | 0.6 | 0.85 | 0.7 |
| Wall slide speed | 1.0 | 0.85 | 1.3 | 0.7 | 1.0 |
| Climb speed | 1.0 | 1.3 | 1.5 | 0.8 | 0.8 |
| Swim speed | 1.0 | 1.2 | 1.4 | 0.85 | 0.8 |
| Turnaround skid duration (frames) | 4 | 3 | 2 | 5 | 6 |
| Wall jump input lockout (frames) | 8 | 5 | 4 | 10 | 8 |

**Transition behavior**: When threat context changes, the parameter multipliers lerp from current values to target values over 10 frames (167ms). Exception: transition TO PANIC is instant (0 frames) -- the adrenaline spike is immediate. Transition FROM PANIC to any other context lerps over 20 frames (333ms) -- the comedown is gradual.

**Context determination priority** (highest to lowest): SHELTERED > PANIC > PRECISE > TENSE > CALM. If multiple conditions are true simultaneously, the highest-priority context wins. Example: a predator in pursuit near a traversal challenge zone triggers PANIC, not PRECISE.

**State-locked context override**: The SQUEEZING movement state forces PRECISE context regardless of the priority table above. Even if a predator is in active pursuit (which would normally produce PANIC), the squeeze passage geometry constrains the creature to PRECISE. This is an explicit exception — the passage's physics do not care about the creature's adrenaline (Pillar 1). Implementation: check movement state before the priority table. If `current_state == SQUEEZING`, set context to PRECISE and skip the priority evaluation.

**Ownership**: The Player Controller owns the feel layer computation. It reads `threat_context` from the World/Biome system (which aggregates predator proximity from Creature AI, hazard data from Environmental Hazards, and zone flags from level geometry). The Player Controller does not determine threat -- it only responds to it.

---

#### CR-7. Surface Interaction

Every tilemap tile carries a `surface_type` enum in its metadata. The Player Controller reads this on contact and applies friction and behavioral modifiers. Surfaces affect ground movement, wall interaction, and climbing.

**Surface type table**:

| Surface Type | Friction Mult (decel) | Accel Mult | Slide Speed Mult | Climbable | Wall-Interactive | Special Behavior |
|---|---|---|---|---|---|---|
| Dry Stone | 1.0 | 1.0 | 1.0 | No | Yes | Baseline. |
| Wet Stone | 0.6 | 0.9 | 1.3 | No | Yes | Deceleration is 60% effective -- creature slides further when stopping or turning. |
| Metal (dry) | 0.8 | 1.0 | 1.0 | No | No | Smooth -- no wall interaction unless evolution provides grip. |
| Metal (wet) | 0.4 | 0.85 | 1.5 | No | No | Very slippery. Stops take 2.5x as long. |
| Moss | 1.3 | 1.0 | 0.8 | No | Yes | High grip. Stops are crisp. Slightly slower wall slide (moss cushions). |
| Dense Vine | 1.0 | 1.0 | 0.9 | Yes | Yes | Climbable surface. Baseline climbing speed. |
| Wet Vine | 0.7 | 0.9 | 1.1 | Yes | Yes | Climbable but at 0.7x climb speed. Ground is slippery. |
| Fungal Mat | 1.1 | 0.9 | 0.7 | No | Yes | Slight bounce on landing (10% velocity reflection). Soft deceleration feel. |
| Root Structure | 1.2 | 1.0 | 0.9 | Yes | Yes | Rigid. Good handholds. 1.1x climb speed. |
| Ice | 0.2 | 0.5 | 2.0 | No | No | Extremely low friction. Acceleration is halved. Slide speed doubled. No grip. |
| Crumbling | 1.0 | 1.0 | 1.0 | No | Yes | Tile breaks after 0.5s of continuous contact. Respawns after 4.0s. |
| Rope/Cable | 1.0 | 1.0 | 1.0 | Yes | No | Swaying climb (see CR-5). Not wall-interactive (too thin for sliding). |

**Surface detection**: On every physics frame, the Player Controller reads `surface_type` from the tile directly below the creature (for ground) and from the tile the creature is pressing against (for walls). If the creature spans two tiles, the tile with the most contact area (>50% of collider overlap) determines the active surface. If exactly 50/50, the lower-friction surface wins (the slippery tile dominates -- conservative for the player).

**Evolution override**: The Biological Evolution system can override surface interaction rules. Example: the "hardened foot pads" mutation sets a minimum friction multiplier of 0.8 (ice and wet metal become manageable). The "gecko pads" mutation makes metal surfaces wall-interactive. These overrides are read from the evolution interface (CR-9) and applied after the base surface lookup.

---

#### CR-8. Crawling (Low Clearance Passages)

**Initiation**: The player presses Down while on the floor AND the creature is in Idle, Walking, or Running state. The collider reshapes from 10x12 to 12x6 over 4 frames (67ms). If there is not enough vertical clearance above for the standing collider (checked via a ShapeCast upward), the creature cannot stand up and remains in crawling state.

**Physics behavior**:
- Crawl speed: 40 px/s
- Crawl acceleration: 300 px/s^2
- Crawl deceleration: 600 px/s^2
- No jumping while crawling. The creature must stand up first (requires clearance).
- The creature can drop off ledges while crawling (transitions to Falling with crawl collider active).

**Forced crawl zones**: Some passages have ceilings low enough that the creature cannot stand. In these zones, the standing-up ShapeCast always fails, keeping the creature locked in crawl until the passage opens. The creature can still interact with one-way platforms (drop through) and trigger squeezing (CR-8b) if the passage narrows further.

**CR-8b. Squeeze/Press (Tight Gap Traversal)**:
- **Initiation**: While crawling, the creature enters a passage narrower than the crawl collider (12px wide). The collider compresses further to 8x4 over 6 frames (100ms). Speed drops to 20 px/s. The creature can only move forward (in the direction it entered) or backward. No vertical movement. No jumping.
- **Physics**: Squeeze speed is 20 px/s, constant (no acceleration curve -- the creature inches forward). Deceleration is instant (0 frames to stop).
- **Termination**: The passage widens enough for the crawl collider (creature expands back to crawl) or the standing collider (creature stands up). Expansion always checks clearance before reshaping.
- **Feel layer interaction**: Squeezing is always PRECISE context regardless of external threats. Even if a predator is in pursuit, the creature cannot squeeze faster. This is a deliberate tension design -- the player commits to a squeeze knowing they are vulnerable.
- **Reversal**: The player can reverse direction while squeezing by pressing the opposite horizontal input. The creature pauses for 4 frames (67ms turnaround) then moves backward at squeeze speed.

**Input mapping**: Down (to enter crawl from standing), horizontal axis (to move while crawling/squeezing). Standing up: release Down when clearance is available.

---

#### CR-9. Evolution Interface (Hooks for Biological Evolution System)

The Player Controller does not own evolution logic. It exposes a set of hooks that the Biological Evolution system writes to, modifying the controller's behavior. Each hook is a named parameter or override slot that the evolution system populates when a mutation is acquired.

**Hook table** (the evolution system writes, the player controller reads):

| Hook Name | Type | Default | What It Modifies |
|---|---|---|---|
| `collider_dimensions` | Vector2 | (10, 12) | Capsule collider width and height. Evolution may grow the creature. |
| `crawl_collider_dimensions` | Vector2 | (12, 6) | Crawl collider width and height. |
| `squeeze_collider_dimensions` | Vector2 | (8, 4) | Squeeze collider width and height. |
| `max_run_speed_bonus` | float | 0.0 | Added to base max run speed (px/s) before feel layer multiplier. |
| `jump_impulse_bonus` | float | 0.0 | Added to base jump impulse magnitude before feel layer multiplier. |
| `gravity_multiplier` | float | 1.0 | Multiplied with all gravity values. Gliding membranes set this to 0.5 while airborne. |
| `terminal_velocity_override` | float | -1.0 | If >= 0, replaces the default terminal velocity. Gliding sets this to 150 px/s. |
| `swim_speed_bonus` | float | 0.0 | Added to base swim speed. Gill-lungs mutation increases this. |
| `swim_oxygen_multiplier` | float | 1.0 | Multiplied with base oxygen duration. Gill-lungs set this to 3.0. |
| `surface_friction_minimum` | float | 0.0 | If > 0, clamps surface friction multiplier to this minimum. Hardened foot pads set this to 0.8. |
| `wall_interactive_override` | Array[String] | [] | List of surface types that become wall-interactive for this creature. Gecko pads add "metal_dry", "metal_wet". |
| `climbable_override` | Array[String] | [] | List of surface types that become climbable. |
| `wall_slide_duration_bonus` | float | 0.0 | Added to the base 2.0s wall slide duration limit. |
| `coyote_time_bonus_frames` | int | 0 | Added to base coyote time frames. |
| `wall_jump_fatigue_reduction` | float | 0.0 | Subtracted from the 20% per-jump fatigue. 0.1 means only 10% fatigue per jump. |
| `can_wall_cling` | bool | true | Some mutations may disable wall cling (bulk). Default is true. |
| `water_movement_mode` | enum | SURFACE | SURFACE (default swim), SUBMERGED (gill-lungs, full underwater), AMPHIBIOUS (both). |
| `hazard_resistances` | Dictionary | {} | Map of hazard_type -> resistance_float. Read by Environmental Hazards, but the Player Controller applies movement slowdowns from unresisted hazards. |
| `passive_speed_modifiers` | Array[{context, param, value}] | [] | Arbitrary additional feel layer modifications from evolution. Applied after the base feel layer table. |

**Contract**: The evolution system writes to these hooks via a shared resource (EvolutionState autoload or a resource attached to the player node). The Player Controller reads them every frame. The Player Controller never writes to these hooks. If a hook value is invalid (negative collider dimension, friction minimum > 1.0), the Player Controller clamps it to the valid range and emits a warning signal.

---

#### CR-10. Swimming (Water Traversal)

Water is a volume defined by Area2D regions in the tilemap. The Player Controller detects water entry and exit via `area_entered` and `area_exited` signals from a detector area on the creature.

**Initiation**: The creature's detector area overlaps with a water volume. Transition to Swimming state is automatic (no input required).

**Water surface vs. submerged**:
- **Surface swimming** (default): The creature floats at the water surface line. The top 30% of the sprite is above water. Movement is horizontal only (left/right). Pressing Down submerges if the evolution `water_movement_mode` is SUBMERGED or AMPHIBIOUS. If the mode is SURFACE, pressing Down has no effect (the creature bobs back up).
- **Submerged swimming**: Full 2D movement within the water volume. Available only with appropriate evolution. Pressing Up near the surface returns to surface swimming.

**Physics behavior (surface)**:
- Swim speed: 70 px/s horizontal
- Swim acceleration: 350 px/s^2
- Swim deceleration: 500 px/s^2
- Jump from water surface: impulse of -200 px/s (weaker than ground jump -- water drag)
- No wall interaction while swimming. Walls in water are just walls.

**Physics behavior (submerged)**:
- Swim speed: 55 px/s in all directions
- Swim acceleration: 300 px/s^2
- Swim deceleration: 400 px/s^2
- Gravity in water: 60 px/s^2 (gentle sink when no input)
- Buoyancy: When no vertical input, the creature slowly rises at 20 px/s toward the surface. Buoyancy and gravity partially cancel: net downward force is 40 px/s^2 only when pressing Down, net upward drift of 20 px/s when neutral.
- Diagonal movement is normalized to the swim speed magnitude.

**Oxygen**: Without gill-lung evolution, the creature has 5.0 seconds of oxygen when submerged (not on the surface). Oxygen depletes at 1.0 per second. At 0 oxygen, the creature takes damage at 1 HP per second. The Diegetic UI system reads oxygen state from the controller and communicates it through visual cues (bubble frequency from the sprite). The `swim_oxygen_multiplier` hook from evolution multiplies the base 5.0s duration.

**Water exit**: The creature exits water by jumping from the surface (lands on adjacent ground) or by swimming to the edge of the water volume where ground is available (auto-climb-out animation, 6 frames, 100ms). If the water volume is enclosed (no exit), the creature must jump.

**Feel layer interaction**: Feel layer applies to swim speed and acceleration via the swim-specific rows in the feel layer table (CR-6).

**Input mapping**: Horizontal axis (left/right movement), Down (submerge if evolution allows), Up (surface from submerged), Jump (jump from surface).

---

#### CR-11. Falling and Landing

**Falling**: Any time the creature is airborne and vertical velocity is positive (moving downward), the creature is in the Falling state (unless in a wall interaction or swimming state).

**Physics**:
- Descending gravity: 1000 px/s^2 (heavier than ascending -- see CR-3)
- Terminal velocity: 350 px/s (capped)
- Air control while falling: same as jumping (70% of ground acceleration at baseline)

**Landing recovery**:
The severity of landing depends on fall duration and velocity at impact.

| Fall Duration | Impact Velocity | Recovery | Effect |
|---|---|---|---|
| < 0.3s | < 200 px/s | Soft landing | 0 recovery frames. Creature transitions immediately to Idle/Walk/Run. No penalty. |
| 0.3s - 0.6s | 200-300 px/s | Medium landing | 4-frame (67ms) landing animation (creature absorbs impact). Player can buffer inputs during these frames. No functional delay. |
| 0.6s - 1.0s | 300-350 px/s | Hard landing | 10-frame (167ms) recovery. Creature stumbles. Horizontal velocity is zeroed. The player cannot input movement for 10 frames but CAN buffer a jump (jump executes on frame 11). |
| > 1.0s | 350 px/s (terminal) | Stagger landing | 18-frame (300ms) recovery. Creature visibly staggers. Horizontal velocity zeroed. 1 HP damage. No input accepted for 18 frames. Jump buffer window starts on frame 12 (buffer is checked on frame 18). |

**Fall damage**: Only stagger landings (>1.0s fall time) deal damage. All other landings are non-damaging. This means most normal jumps (apex at 0.35s, total air time ~0.7s) result in soft or medium landings. Only long falls from significant heights punish.

**Ledge detection**: When falling past a ledge the creature could grab, the controller does NOT auto-grab. The player must input toward the wall to initiate wall-slide. Auto-grab would violate the "mass and commitment" principle -- the creature is a body in freefall, not a magnet.

**Feel layer interaction**: In PANIC context, hard landing recovery is reduced to 7 frames and stagger recovery to 14 frames (adrenaline cushions the impact). In PRECISE context, recovery frames are unchanged but the buffer windows open 2 frames earlier.

**Input mapping**: No dedicated falling input. Air control via horizontal axis. Landing is automatic on surface contact.

---

#### CR-12. Additional Ground Rules

**CR-12a. Ledge assist**: When the creature would miss a platform by 2px or fewer horizontally, the controller nudges the creature's position by up to 2px to land on the platform. This is invisible to the player but prevents frustrating near-misses. The nudge only applies to the top corners of platform edges, not to walls.

**CR-12b. Slope handling**: Slopes up to 45 degrees are traversable at full ground speed. Slopes between 46-60 degrees reduce speed by 50% uphill and increase speed by 25% downhill. Slopes above 60 degrees are treated as walls (wall-slide activates). The creature does not slide down slopes when idle on any traversable slope (Godot's `floor_snap_length` is set to 8px to keep the creature grounded).

**CR-12c. Moving platforms**: The creature inherits the velocity of any moving platform it stands on. When the creature jumps from a moving platform, the platform's velocity is added to the jump velocity (momentum preservation). This applies to moving vine structures, floating debris in water, and any Area2D-flagged kinematic body.

**CR-12d. External forces**: External forces (wind, water current, creature knockback, explosion push) are applied as additive velocity vectors that bypass the feel layer. External forces decay at their own rate (specified by the source system). The Player Controller applies them in `move_and_slide()` alongside the creature's own velocity but does not modify them. When an external force exceeds 200 px/s magnitude, the creature enters the Stunned state for the force's specified stun duration.

**CR-12e. Head bonking**: If the creature hits a ceiling while ascending, vertical velocity is immediately set to 0 (upward movement stops). The creature transitions to Falling. There is no stun or damage from head bonking.

---

### States and Transitions

The movement state machine uses a flat (non-hierarchical) FSM. Each state is mutually exclusive. The active state determines which physics rules from Core Rules are applied on each frame. State transitions are checked every physics frame in priority order (highest-priority transitions checked first).

**State enum values**: IDLE, WALKING, RUNNING, JUMPING, FALLING, WALL_SLIDING, WALL_CLINGING, WALL_JUMPING, CLIMBING, CRAWLING, SQUEEZING, SWIMMING_SURFACE, SWIMMING_SUBMERGED, LANDING_RECOVERY, STUNNED, DEAD

**Transition table** (checked every physics frame, first matching condition wins):

| # | From State | To State | Condition | Priority | Notes |
|---|---|---|---|---|---|
| 1 | ANY | DEAD | `health <= 0` | 100 | Highest priority. Overrides everything. Triggers death sequence. |
| 2 | ANY except DEAD | STUNNED | External force > 200 px/s received | 99 | Force-triggered stun. Duration set by force source. |
| 3 | ANY except DEAD, STUNNED | SWIMMING_SURFACE | Creature enters water volume AND `water_movement_mode != SUBMERGED` | 98 | Water entry overrides all non-death/stun states. |
| 4 | ANY except DEAD, STUNNED | SWIMMING_SUBMERGED | Creature enters water volume AND `water_movement_mode == SUBMERGED` | 98 | Creatures with submerged-only mode go straight to submerged. |
| 5 | STUNNED | IDLE | Stun timer expires AND `is_on_floor()` | 95 | Recovery from stun while grounded. |
| 6 | STUNNED | FALLING | Stun timer expires AND NOT `is_on_floor()` | 95 | Recovery from stun while airborne. |
| 7 | IDLE | CRAWLING | Down input AND `is_on_floor()` | 90 | Enter crawl from idle. Collider reshapes over 4 frames. |
| 8 | WALKING | CRAWLING | Down input AND `is_on_floor()` | 90 | Enter crawl from walk. |
| 9 | RUNNING | CRAWLING | Down input AND `is_on_floor()` | 90 | Enter crawl from run. Horizontal velocity decelerates during collider reshape. |
| 10 | CRAWLING | SQUEEZING | Creature enters a passage narrower than crawl collider | 88 | Auto-transition when geometry forces it. |
| 11 | SQUEEZING | CRAWLING | Passage widens to crawl collider clearance | 87 | Auto-transition when geometry allows. |
| 12 | CRAWLING | IDLE | Release Down AND clearance for standing collider AND no horizontal input | 85 | Stand up from crawl. Collider reshapes over 4 frames. |
| 13 | CRAWLING | WALKING | Release Down AND clearance for standing collider AND horizontal input (magnitude < 0.4) | 85 | Stand up into walk. |
| 14 | CRAWLING | RUNNING | Release Down AND clearance for standing collider AND horizontal input (magnitude >= 0.4) | 85 | Stand up into run. |
| 15 | CRAWLING | FALLING | Floor disappears beneath crawling creature | 84 | Crawl off ledge. Keeps crawl collider until landing. |
| 16 | IDLE | JUMPING | Jump input AND (`is_on_floor()` OR coyote_timer > 0) | 80 | Ground jump from standstill. |
| 17 | WALKING | JUMPING | Jump input AND (`is_on_floor()` OR coyote_timer > 0) | 80 | Jump from walk. Preserves horizontal velocity. |
| 18 | RUNNING | JUMPING | Jump input AND (`is_on_floor()` OR coyote_timer > 0) | 80 | Jump from run. Preserves horizontal velocity. |
| 19 | WALL_SLIDING | WALL_JUMPING | Jump input | 78 | Wall jump. See CR-4c for impulse values. |
| 20 | WALL_CLINGING | WALL_JUMPING | Jump input | 78 | Wall jump from cling. Same impulse as from slide. |
| 21 | CLIMBING | JUMPING | Jump input | 78 | Launch from climb surface. See CR-5 for impulse values. |
| 22 | SWIMMING_SURFACE | JUMPING | Jump input | 78 | Water surface jump. Weaker impulse (CR-10). |
| 23 | SWIMMING_SURFACE | SWIMMING_SUBMERGED | Down input AND `water_movement_mode` is SUBMERGED or AMPHIBIOUS | 75 | Dive beneath surface. |
| 24 | SWIMMING_SUBMERGED | SWIMMING_SURFACE | Up input AND creature at water surface line | 75 | Surface from dive. |
| 25 | SWIMMING_SURFACE | IDLE | Creature reaches water edge with adjacent ground AND horizontal input toward ground | 73 | Auto climb-out. 6-frame animation. |
| 26 | SWIMMING_SUBMERGED | IDLE | Creature reaches water edge with adjacent ground AND horizontal input toward ground | 73 | Climb out from submerged. |
| 27 | WALL_JUMPING | FALLING | 8 frames elapsed since wall jump AND vertical velocity >= 0 | 70 | Wall jump arc complete, now falling. |
| 28 | WALL_JUMPING | WALL_SLIDING | 8 frames elapsed AND creature contacts wall-interactive surface AND horizontal input toward wall | 70 | Chain wall jump into wall slide on a different wall. |
| 29 | JUMPING | FALLING | Vertical velocity >= 0 (apex reached) | 68 | Jump apex transitions to fall. |
| 30 | JUMPING | WALL_SLIDING | Creature contacts wall-interactive surface AND horizontal input toward wall AND vertical velocity >= 0 | 67 | Jump into wall slide (only after apex to prevent accidental wall grabs during the upward arc). |
| 31 | FALLING | WALL_SLIDING | Creature contacts wall-interactive surface AND horizontal input toward wall | 65 | Fall into wall slide. |
| 32 | FALLING | CLIMBING | Creature contacts climbable surface AND grab input active | 64 | Grab a climbable surface while falling. |
| 33 | WALL_SLIDING | WALL_CLINGING | Grab input AND surface has `wall_cling_allowed` | 63 | Cling from slide. Vertical velocity zeroes instantly. |
| 34 | WALL_CLINGING | WALL_SLIDING | Release grab input | 62 | Release cling, resume sliding. |
| 35 | WALL_SLIDING | FALLING | Release horizontal input toward wall OR wall surface ends | 60 | Detach from wall. |
| 36 | WALL_CLINGING | FALLING | Down input (intentional drop) | 60 | Drop from cling. No coyote time. |
| 37 | CLIMBING | FALLING | Release grab input | 58 | Detach from climbable surface. |
| 38 | CLIMBING | IDLE | Reach top of climbable surface with floor above | 57 | Auto-mantle. 4-frame animation. |
| 39 | CLIMBING | WALL_SLIDING | Climbable surface ends but underlying wall is wall-interactive | 56 | Transition from climb to wall slide when vines end but wall continues. |
| 40 | FALLING | LANDING_RECOVERY | `is_on_floor()` AND impact qualifies as hard or stagger landing (CR-11) | 55 | Hard landing recovery. |
| 41 | FALLING | IDLE | `is_on_floor()` AND impact is soft landing AND no horizontal input | 54 | Soft/medium landing to idle. |
| 42 | FALLING | WALKING | `is_on_floor()` AND impact is soft/medium landing AND horizontal input (< 0.4) | 54 | Land into walk. |
| 43 | FALLING | RUNNING | `is_on_floor()` AND impact is soft/medium landing AND horizontal input (>= 0.4) | 54 | Land into run. |
| 44 | FALLING | CRAWLING | `is_on_floor()` AND crawl collider was active during fall | 54 | Land from a crawl-ledge-fall. Stay crawling. |
| 45 | FALLING | SWIMMING_SURFACE | Creature enters water volume during fall | 53 | Fall into water. Small splash. |
| 46 | LANDING_RECOVERY | IDLE | Recovery timer expires AND no horizontal input | 50 | Stand up after hard/stagger landing. |
| 47 | LANDING_RECOVERY | WALKING | Recovery timer expires AND horizontal input (< 0.4) | 50 | Recover into walk. |
| 48 | LANDING_RECOVERY | RUNNING | Recovery timer expires AND horizontal input (>= 0.4) | 50 | Recover into run. |
| 49 | LANDING_RECOVERY | JUMPING | Recovery timer expires AND jump was buffered during recovery | 50 | Buffered jump executes on recovery frame. |
| 50 | IDLE | WALKING | Horizontal input (magnitude < 0.4) | 40 | Begin walking. |
| 51 | IDLE | RUNNING | Horizontal input (magnitude >= 0.4) | 40 | Begin running. |
| 52 | WALKING | RUNNING | Horizontal input magnitude >= 0.4 | 38 | Accelerate to run. No transition animation -- speed just increases. |
| 53 | RUNNING | WALKING | Horizontal input magnitude < 0.4 | 38 | Decelerate to walk. |
| 54 | WALKING | IDLE | Horizontal input released AND velocity < 5 px/s | 35 | Stop walking. Velocity threshold prevents premature idle on slow decel. |
| 55 | RUNNING | IDLE | Horizontal input released AND velocity < 5 px/s | 35 | Stop running. |
| 56 | IDLE | FALLING | NOT `is_on_floor()` AND coyote_timer <= 0 | 30 | Walk off ledge after coyote expires. |
| 57 | WALKING | FALLING | NOT `is_on_floor()` AND coyote_timer <= 0 | 30 | Walk off ledge. |
| 58 | RUNNING | FALLING | NOT `is_on_floor()` AND coyote_timer <= 0 | 30 | Run off ledge. |

**State entry actions** (executed once on transition):

| State | Entry Action |
|---|---|
| IDLE | Set horizontal velocity to 0 (after deceleration completes in previous state). Reset coyote timer if just left a floor. |
| JUMPING | Apply jump impulse. Consume coyote timer. Consume jump buffer. |
| WALL_SLIDING | Set wall_slide_timer to 0. Record which wall (left/right). |
| WALL_CLINGING | Set vertical velocity to 0. |
| WALL_JUMPING | Apply wall jump impulse (vertical + horizontal). Increment wall_jump_fatigue_counter. Start input lockout timer. |
| CLIMBING | Set gravity to 0. Zero velocity. Enter climb movement mode. |
| CRAWLING | Begin collider reshape (10x12 to 12x6 over 4 frames). |
| SQUEEZING | Begin collider reshape (12x6 to 8x4 over 6 frames). Set speed to squeeze speed. Lock vertical movement. |
| SWIMMING_SURFACE | Set gravity to 0. Snap creature to water surface line. Zero vertical velocity. |
| SWIMMING_SUBMERGED | Set gravity to 60 px/s^2. Start oxygen timer (if applicable). |
| LANDING_RECOVERY | Zero horizontal velocity. Start recovery timer based on impact severity (CR-11). |
| STUNNED | Zero all velocity. Start stun timer (duration from external force). Emit `stunned` signal. |
| DEAD | Zero all velocity. Disable collision. Emit `died` signal. Begin death animation. After animation (30 frames, 500ms), emit `request_respawn` signal. |

**State exit actions**:

| State | Exit Action |
|---|---|
| WALL_SLIDING | Reset wall_slide_timer. |
| CLIMBING | Restore normal gravity. |
| CRAWLING | Begin collider reshape back to standing (12x6 to 10x12 over 4 frames) if transitioning to a standing state. |
| SQUEEZING | Begin collider reshape back to crawl (8x4 to 12x6 over 6 frames). |
| SWIMMING_SURFACE | Restore normal gravity. |
| SWIMMING_SUBMERGED | Restore normal gravity. Stop oxygen timer. |
| STUNNED | Emit `stun_recovered` signal. |

**Coyote time management**: When the creature leaves the floor without jumping (transitions 56-58), start a coyote timer counting down from 6 frames. If jump is pressed before the timer expires, execute a ground jump (transition 16-18 condition met via coyote_timer > 0). Coyote timer is reset on any floor contact.

**Jump buffer management**: When jump is pressed while not on a jumpable surface, start a 6-frame buffer. If the creature contacts a floor or wall (for wall jump) within those frames, execute the appropriate jump on contact.

---

### Interactions with Other Systems

#### INT-1. Input System

| Aspect | Detail |
|---|---|
| **Direction** | Input System -> Player Controller (upstream dependency) |
| **Interface owner** | Input System |
| **Data consumed by Player Controller** | `move_axis: Vector2` (horizontal: -1.0 to 1.0, vertical: -1.0 to 1.0, analog magnitude preserved); `jump_pressed: bool` (true on the frame jump is pressed); `jump_held: bool` (true while jump is held); `jump_released: bool` (true on the frame jump is released); `grab_pressed: bool` (true on frame grab input is pressed); `grab_held: bool` (true while grab is held); `grab_released: bool` (true on frame grab is released); `down_pressed: bool` (true on frame Down is pressed); `down_held: bool` (true while Down is held) |
| **Data provided by Player Controller** | None. The Input System does not read from the Player Controller. |
| **Contract** | The Input System provides a normalized, device-agnostic action interface. The Player Controller never reads raw device input (no `Input.is_key_pressed()` calls). All input comes through action names mapped in the Input System. The Player Controller trusts that `move_axis` is a valid Vector2 and that boolean actions reflect debounced, clean signals. |

---

#### INT-2. Camera System

| Aspect | Detail |
|---|---|
| **Direction** | Player Controller -> Camera System (downstream dependency) |
| **Interface owner** | Player Controller (provides data), Camera System (consumes and acts) |
| **Data provided by Player Controller** | `global_position: Vector2` (creature's world position, read every frame); `velocity: Vector2` (creature's current velocity, used for look-ahead); `current_state: StateEnum` (the active movement state); `facing_direction: int` (-1 for left, 1 for right); `threat_context: ThreatContext` (current feel layer context, so camera can adjust framing per the art bible mood targets) |
| **Data consumed by Player Controller** | None. The Player Controller does not read camera state. Camera is purely reactive to player state. |
| **Contract** | The Camera System reads the Player Controller's exported properties every frame. The Player Controller does not call any Camera methods. The Camera is responsible for smoothing, look-ahead, screen transitions, and parallax management based on the data it reads. The art bible specifies camera behavior per mood state (depth collapse during pursuit, parallax breath during calm) -- the Camera System uses `threat_context` to drive these behaviors. |

---

#### INT-3. Biological Evolution System

| Aspect | Detail |
|---|---|
| **Direction** | Bidirectional. Evolution writes parameters to the Player Controller's hook table (CR-9). Player Controller reads those parameters every frame. Player Controller provides state data to Evolution for mutation triggers. |
| **Interface owner** | Shared. The hook table schema (CR-9) is owned by the Player Controller GDD. The mutation trigger conditions are owned by the Biological Evolution GDD. |
| **Data provided by Player Controller** | `current_state: StateEnum`; `current_surface: SurfaceType`; `time_in_state: float` (how long the creature has been in the current state, used for survival-time triggers); `cumulative_biome_exposure: Dictionary` (map of biome_id -> seconds spent, updated every frame the creature is in that biome); `damage_sources: Array` (list of hazard/damage types taken since last shelter visit, used for adaptation triggers); `is_alive: bool` |
| **Data consumed by Player Controller** | The entire hook table defined in CR-9. Read every physics frame. |
| **Contract** | The Evolution system writes to the hook table when a mutation is acquired (one-time write per mutation, not per-frame). The Player Controller re-reads the hook table every frame (cheap, since it is local data). The Evolution system MUST NOT modify the Player Controller's velocity, state, or position directly -- it can only modify the parameters that the controller's own physics simulation uses. If a mutation requires an immediate state change (e.g., forced swim mode change while already in water), the Evolution system sets the hook and emits a `mutation_applied` signal. The Player Controller listens for this signal and re-evaluates its current state on the next frame. |

---

#### INT-4. Shelter & Rest System

| Aspect | Detail |
|---|---|
| **Direction** | Bidirectional. Player Controller provides position data. Shelter & Rest provides zone data and respawn coordinates. |
| **Interface owner** | Shelter & Rest System (owns shelter zone definitions and respawn logic). Player Controller provides respawn interface. |
| **Data provided by Player Controller** | `global_position: Vector2`; `current_state: StateEnum`; `is_alive: bool`; `request_respawn: Signal` (emitted when death animation completes) |
| **Data consumed by Player Controller** | `is_in_shelter: bool` (used by the feel layer to set SHELTERED context); `last_shelter_position: Vector2` (respawn coordinates); `respawn_state: Dictionary` (health, oxygen, and any state resets to apply on respawn) |
| **Contract** | The Shelter & Rest system detects when the player enters a shelter via Area2D overlap (it has its own detection, separate from the Player Controller). It sets `is_in_shelter` to true, which the Player Controller's feel layer reads to activate SHELTERED context. On death, the Player Controller emits `request_respawn`. The Shelter & Rest system handles the screen transition and then calls `respawn(position, state)` on the Player Controller. The `respawn()` method: sets position to `last_shelter_position`, resets velocity to zero, resets state to IDLE, applies `respawn_state` values (full health, full oxygen), and enables collision. The Player Controller does not own respawn logic beyond applying the provided state. |

---

#### INT-5. Diegetic UI System

| Aspect | Detail |
|---|---|
| **Direction** | Player Controller -> Diegetic UI (downstream, read-only) |
| **Interface owner** | Player Controller (exposes state). Diegetic UI reads and visualizes. |
| **Data provided by Player Controller** | `health_current: int`; `health_max: int`; `oxygen_current: float` (0.0-1.0 normalized); `oxygen_depleting: bool`; `current_state: StateEnum`; `threat_context: ThreatContext`; `velocity: Vector2` (used for directional stress indicators); `facing_direction: int`; `is_in_shelter: bool`; `evolution_hooks: Dictionary` (reference to the hook table, so UI can reflect mutation effects) |
| **Data consumed by Player Controller** | None. The Diegetic UI is purely a consumer. It does not modify player state. |
| **Contract** | The Diegetic UI reads Player Controller properties every frame to drive sprite degradation tiers (health visualization per art bible: 4 tiers), the dorsal ridge compass (shelter direction derived from `global_position` relative to `last_shelter_position`), oxygen bubble frequency, and any stress indicators. The UI MUST NOT call any Player Controller methods or modify any properties. All communication is read-only. The reflection pool mechanic (evolution status display in shelters) reads the `evolution_hooks` reference. |

---

#### INT-6. Player Visual System

| Aspect | Detail |
|---|---|
| **Direction** | Player Controller -> Player Visual System (downstream, read-only for state; Player Visual owns the sprite node) |
| **Interface owner** | Player Visual System (owns AnimatedSprite2D). Player Controller provides state data. |
| **Data provided by Player Controller** | `current_state: StateEnum`; `velocity: Vector2`; `facing_direction: int`; `threat_context: ThreatContext`; `is_on_floor: bool`; `wall_direction: int` (which side the wall is on during wall states, -1 or 1); `is_crawling: bool`; `is_squeezing: bool`; `collider_reshaping: bool` (true during the frames where the collider is transitioning); `evolution_hooks: Dictionary` (for selecting the correct sprite set based on mutations) |
| **Data consumed by Player Controller** | None. The Player Visual system reads controller state and plays appropriate animations. |
| **Contract** | The Player Visual system selects animation based on `current_state` and `threat_context` (the art bible defines three animation intensity tiers: calm/tense/panic per locomotion state). Animation playback does NOT affect controller state -- animations are cosmetic. If the Player Visual system needs to signal something (e.g., a specific animation frame reached for audio sync), it emits its own signals that other systems (Audio) listen to. The Player Controller is unaware of which animation is playing. Collider shape is owned by the controller, not the visual. The sprite may visually extend beyond the collider (the 16x16 sprite is larger than the 10x12 collider). |

---

#### INT-7. World/Biome System

| Aspect | Detail |
|---|---|
| **Direction** | World/Biome -> Player Controller (upstream data provider); Player Controller -> World/Biome (position reporting) |
| **Interface owner** | World/Biome System (owns tilemap data, zone definitions, surface types) |
| **Data provided by Player Controller** | `global_position: Vector2` (so the World system knows which biome the creature is in); `current_biome_id: StringName` (set by the World system, cached on the controller for other systems to read without querying World directly) |
| **Data consumed by Player Controller** | `surface_type: SurfaceType` (per tile, read from tilemap metadata on contact); `zone_flags: Array[StringName]` (flags on the current zone: "traversal_challenge", "water_volume", "squeeze_passage", etc.); `threat_context: ThreatContext` (aggregated by World/Biome from Creature AI, Environmental Hazards, and zone flags -- the Player Controller reads this single enum, not the raw sources); `biome_gravity_modifier: float` (some biomes may have non-standard gravity, e.g., deep underground fungal caverns with spore updrafts) |
| **Contract** | The World/Biome system is the authority on what the environment IS. The Player Controller is the authority on how the creature MOVES within that environment. The World system provides tile metadata and zone flags. The Player Controller reads them. Neither system modifies the other's primary data. The `threat_context` aggregation is performed by the World/Biome system because it requires data from multiple sources (Creature AI, Hazards, zone geometry) that the Player Controller should not depend on directly. This keeps the Player Controller's dependency list to a single upstream system for environmental data. |

---

#### INT-8. Time & Weather System

| Aspect | Detail |
|---|---|
| **Direction** | Time & Weather -> Player Controller (via World/Biome aggregation; no direct dependency) |
| **Interface owner** | Time & Weather System |
| **Data provided by Player Controller** | None directly. Time & Weather reads player position indirectly through World/Biome. |
| **Data consumed by Player Controller** | `weather_surface_modifier: Dictionary` (map of surface_type -> friction_modifier_override, applied when weather changes surface properties -- rain makes stone wet, frost creates ice patches). Read from World/Biome, which receives it from Time & Weather. `wind_force: Vector2` (applied as an external force per CR-12d, provided via World/Biome). |
| **Contract** | The Player Controller does NOT depend directly on Time & Weather. All weather effects reach the controller through the World/Biome system, which translates weather states into surface modifiers and external forces. This indirection means the Player Controller does not need to know about rain, wind, or temperature -- it only knows about surfaces and forces. When it rains, the World/Biome system changes "dry_stone" tiles in exposed areas to effective "wet_stone" friction values by providing a `weather_surface_modifier` override. The Player Controller applies this override in its surface lookup (CR-7). |

---

#### INT-9. Creature AI System

| Aspect | Detail |
|---|---|
| **Direction** | Creature AI -> Player Controller (indirect, via World/Biome threat aggregation); Player Controller -> Creature AI (position data, indirect) |
| **Interface owner** | Creature AI System (owns creature behavior). World/Biome mediates. |
| **Data provided by Player Controller** | `global_position: Vector2`; `velocity: Vector2`; `current_state: StateEnum`. Creature AI reads these to determine if the player is visible, reachable, or fleeing. This data is accessed via the player node reference, not through a dedicated interface. |
| **Data consumed by Player Controller** | `threat_context: ThreatContext` (via World/Biome aggregation -- the World system reads Creature AI pursuit states and proximity data and produces the threat context enum). The Player Controller does NOT read individual creature states, positions, or behaviors. It does not know which creature is chasing it or how many predators are nearby. It only knows the resulting threat context. |
| **Contract** | The Player Controller and Creature AI system are decoupled. The Creature AI reads the player's public state (position, velocity, movement state) to drive its own behavioral decisions (pursuit, avoidance, detection). The Player Controller reads only the aggregated threat context. This decoupling is critical: the Player Controller must not contain any logic about specific creatures or behaviors. If a new creature type is added, the Player Controller requires zero changes -- the World/Biome system handles the new creature's contribution to threat context. Physical interactions (creature knockback, creature pushing the player) are handled as external forces (CR-12d) that the Creature AI applies to the player's CharacterBody2D directly through Godot's physics. |

---

#### INT-10. Environmental Hazards System

| Aspect | Detail |
|---|---|
| **Direction** | Environmental Hazards -> Player Controller (damage, forces, surface changes); Player Controller -> Environmental Hazards (position, evolution resistances) |
| **Interface owner** | Environmental Hazards System |
| **Data provided by Player Controller** | `global_position: Vector2`; `hazard_resistances: Dictionary` (from evolution hooks CR-9); `current_state: StateEnum` (hazards may affect some states differently -- submerged creatures are immune to airborne toxins) |
| **Data consumed by Player Controller** | `hazard_damage: int` (applied per-frame or per-tick by the hazard system via a `damage(amount, type)` method on the controller); `hazard_forces: Vector2` (applied as external forces per CR-12d -- thermal updrafts, toxic zone repulsion); `hazard_surface_overrides: Dictionary` (some hazards change surface properties -- acid turns stone into a low-friction damage surface) |
| **Contract** | Environmental Hazards detect the player via Area2D overlap. When the player is in a hazard zone, the hazard system calls `damage(amount, type)` on the Player Controller. The controller subtracts from health, checks for death, and emits `damage_taken(amount, type)` signal (consumed by Diegetic UI and Player Visual for feedback). The Player Controller does NOT own hazard logic -- it does not know what a "toxic zone" is. It only receives damage and forces. Hazard resistances from evolution are checked by the Hazard system before it calls `damage()` -- if the creature resists the hazard, the Hazard system reduces or eliminates the damage before sending it. The Player Controller applies the damage it receives without questioning it. |

---

#### INT-11. Narrative Discovery System

| Aspect | Detail |
|---|---|
| **Direction** | Player Controller -> Narrative Discovery (position data); Narrative Discovery -> Player Controller (minimal: interaction prompts) |
| **Interface owner** | Narrative Discovery System |
| **Data provided by Player Controller** | `global_position: Vector2`; `current_state: StateEnum`; `velocity: Vector2`; `is_on_floor: bool` |
| **Data consumed by Player Controller** | `near_discovery_node: bool` (true when the creature is within interaction range of a narrative discovery point); `discovery_interaction_active: bool` (true when the player has engaged with a discovery, during which the Player Controller reduces max speed by 50% but does not lock movement -- the creature can still walk away from a discovery mid-interaction, consistent with Pillar 1: the world does not pause for the player) |
| **Contract** | Narrative Discovery nodes detect the player via proximity (Area2D). When the player is near a node, the Narrative system sets `near_discovery_node` to true. The Player Visual system uses this flag to trigger an observational animation (the creature pauses and looks at the node). If the player presses the interact input (grab button, repurposed contextually), `discovery_interaction_active` becomes true. The Player Controller applies a 50% max speed reduction while this flag is true. The creature does NOT stop moving, enter a cutscene, or lose control -- consistent with Pillar 1. The Narrative system handles all discovery content display. The Player Controller's only responsibility is the speed reduction and providing position data for proximity detection. |

## Formulas

### F-1: Effective Ground Speed

`effective_ground_speed = (base_speed + evo_bonus) * feel_mult * surface_accel_mult * input_magnitude`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| base_speed | `base_speed` | float | 60.0–120.0 px/s | Base walk speed (60) or run speed (120), interpolated by input magnitude (CR-2) |
| evo_bonus | `evo_bonus` | float | 0.0–unbounded | `max_run_speed_bonus` from Evolution hook table (CR-9). Default 0.0 |
| feel_mult | `feel_mult` | float | 0.5–1.33 | Feel layer max run speed multiplier (CR-6): CALM=1.0, TENSE=1.17, PANIC=1.33, PRECISE=0.75, SHELTERED=0.5 |
| surface_accel_mult | `surface_accel_mult` | float | 0.5–1.0 | Surface type acceleration multiplier (CR-7): Dry Stone=1.0, Wet Stone=0.9, Metal Wet=0.85, Ice=0.5 |
| input_magnitude | `input_magnitude` | float | 0.0–1.0 | Analog stick magnitude or keyboard binary (0.0/1.0). Values <0.4 select walk speed, >=0.4 select run speed |

**Output Range:** 0.0 px/s (no input) to ~159.6 px/s (120 base + 0 evo bonus, PANIC 1.33, Dry Stone 1.0, full input). With evolution bonus of +30 px/s, maximum reaches ~199.5 px/s. Minimum non-zero speed: 15.0 px/s (60 walk * 0.5 SHELTERED * 0.5 Ice * 1.0 input, though Ice+SHELTERED is an unlikely combination).
**Example:** Running on dry stone in TENSE context, no evolution bonus: `(120.0 + 0.0) * 1.17 * 1.0 * 1.0 = 140.4 px/s`. Walking on wet stone in CALM context: `(60.0 + 0.0) * 1.0 * 0.9 * 0.39 = 21.06 px/s` (analog stick at 0.39, just under walk threshold, so base_speed is 60).

---

### F-2: Ground Acceleration / Deceleration

`effective_accel = base_accel * feel_accel_mult * surface_accel_mult`

`effective_decel = base_decel * feel_decel_mult * surface_friction_mult`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| base_accel | `base_accel` | float | 600.0 px/s^2 | Ground acceleration from standstill (CR-2) |
| base_decel | `base_decel` | float | 900.0 or 1200.0 px/s^2 | 900 for input-release deceleration, 1200 for input-reversal deceleration (CR-2) |
| feel_accel_mult | `feel_accel_mult` | float | 0.6–1.5 | Feel layer ground acceleration multiplier (CR-6): CALM=1.0, TENSE=1.25, PANIC=1.5, PRECISE=0.9, SHELTERED=0.6 |
| feel_decel_mult | `feel_decel_mult` | float | 0.75–1.3 | Feel layer ground deceleration multiplier (CR-6): CALM=1.0, TENSE=0.9, PANIC=0.75, PRECISE=1.22, SHELTERED=1.3 |
| surface_accel_mult | `surface_accel_mult` | float | 0.5–1.0 | Surface acceleration multiplier (CR-7): Dry Stone=1.0, Ice=0.5, Metal Wet=0.85, Wet Stone=0.9 |
| surface_friction_mult | `surface_friction_mult` | float | 0.2–1.3 | Surface friction (deceleration) multiplier (CR-7): Dry Stone=1.0, Ice=0.2, Wet Metal=0.4, Moss=1.3 |

**Output Range:** Acceleration: 180.0 px/s^2 (600 * 0.6 SHELTERED * 0.5 Ice) to 900.0 px/s^2 (600 * 1.5 PANIC * 1.0 Dry Stone). Deceleration (input-release): 135.0 px/s^2 (900 * 0.75 PANIC * 0.2 Ice) to 1521.0 px/s^2 (1200 * 1.3 SHELTERED * 0.975 -- note: SHELTERED + reversal deceleration on Moss = 1200 * 1.3 * 1.3 = 2028.0 px/s^2, effectively instant stops).
**Example:** Running on wet stone in PANIC, player releases input: `900.0 * 0.75 * 0.6 = 405.0 px/s^2`. Time to stop from 159.6 px/s (PANIC run speed on wet stone): `159.6 / 405.0 = 0.394s` -- the creature slides nearly half a second. Contrast with PRECISE on moss, input released: `900.0 * 1.22 * 1.3 = 1427.4 px/s^2`. Time to stop from 90 px/s: `90.0 / 1427.4 = 0.063s` -- near-instant.

---

### F-3: Jump Height (Variable)

**Full jump height** (hold to apex):

`jump_height_full = impulse^2 / (2 * gravity_ascend)`

**Short hop height** (release before apex; velocity cut to 40%):

`jump_height_short = (v_release^2 - v_cut^2) / (2 * gravity_ascend) + v_cut^2 / (2 * gravity_ascend)`

Simplifies to: `jump_height_short = v_release^2 / (2 * g) - 0.84 * (v_remaining_at_release^2) / (2 * g)` -- but more precisely, the creature rises at full impulse until the player releases, then velocity is multiplied by 0.4, and the creature continues to rise at that reduced velocity until it reaches zero.

The general form:

`jump_height = h_before_release + h_after_release`

Where:
- `h_before_release = v_release * t_hold - 0.5 * g * t_hold^2` (distance risen before button release)
- `v_at_release = impulse - g * t_hold` (velocity at the moment of release)
- `v_cut = v_at_release * 0.4` (velocity after the cut)
- `h_after_release = v_cut^2 / (2 * g)` (remaining rise after cut)

For full hold (no release): `t_hold = impulse / g` (time to apex), so `h_before_release = impulse^2 / (2 * g)` and `h_after_release = 0`.

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| impulse | `impulse` | float | 238.0–322.0 px/s | Magnitude of initial upward velocity. Base: 280 (CR-3). With feel layer: 280 * feel_jump_mult. With evo: (280 + jump_impulse_bonus) * feel_jump_mult. CALM=1.0, TENSE=1.07, PANIC=1.15, PRECISE=1.0, SHELTERED=0.85 |
| gravity_ascend | `g` | float | 576.0–880.0 px/s^2 | Ascending gravity. Base: 800 (CR-3). With feel layer: 800 * feel_grav_mult * evo_gravity_mult. CALM=1.0, TENSE=1.0, PANIC=1.1, PRECISE=0.9, SHELTERED=1.0. Evo gravity_multiplier default 1.0 |
| t_hold | `t_hold` | float | 0.0–0.35s | Duration the player holds the jump button. 0 = instant tap release. Max = time to apex (impulse / g) |
| cut_multiplier | `cut_mult` | float | 0.4 | Fixed. Remaining upward velocity is multiplied by this on early release (CR-3) |

**Output Range:** Minimum (instant tap, SHELTERED): impulse = 280 * 0.85 = 238, g = 800. On instant release (t_hold ~ 1 frame = 0.0167s): h_before = 238 * 0.0167 - 0.5 * 800 * 0.0167^2 = 3.87 px. v_at_release = 238 - 800 * 0.0167 = 224.64. v_cut = 224.64 * 0.4 = 89.86. h_after = 89.86^2 / (2 * 800) = 5.05 px. Total: ~8.9 px. Maximum (full hold, PANIC, no evo): impulse = 280 * 1.15 = 322, g = 800 * 1.1 = 880. height = 322^2 / (2 * 880) = 58.9 px (~3.7 tiles). With gliding membrane (evo gravity_mult = 0.5): g = 400, height = 280^2 / (2 * 400) = 98.0 px (~6.1 tiles).
**Example:** CALM context, full hold, no evo bonus. impulse = 280, g = 800. `height = 280^2 / (2 * 800) = 78400 / 1600 = 49.0 px` (3.06 tiles). Time to apex: `280 / 800 = 0.35s`. This matches CR-3 exactly. Short hop (release at t=0.1s): h_before = 280 * 0.1 - 0.5 * 800 * 0.01 = 28.0 - 4.0 = 24.0 px. v_at_release = 280 - 80 = 200. v_cut = 200 * 0.4 = 80. h_after = 80^2 / 1600 = 4.0 px. Total: 28.0 px (~1.75 tiles).

---

### F-4: Coyote Time Window

`coyote_valid = (frames_since_floor_left <= coyote_frames) AND (left_floor_without_jumping == true)`

`coyote_frames = base_coyote + evo_coyote_bonus`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| frames_since_floor_left | `frames_off` | int | 0–unbounded | Physics frames elapsed since creature last had `is_on_floor() == true`. Increments by 1 per physics frame |
| base_coyote | `base_coyote` | int | 6 | Base coyote time: 6 frames = 100ms at 60fps (CR-3a) |
| evo_coyote_bonus | `evo_coyote_bonus` | int | 0–unbounded | `coyote_time_bonus_frames` from Evolution hook table (CR-9). Default 0 |
| left_floor_without_jumping | `no_jump_exit` | bool | true/false | True only if the creature walked or ran off a ledge. False if the creature jumped, was pushed by external force, or released from a wall (CR-3a) |

**Output Range:** Boolean. The window is 6 frames (100ms) at baseline. With an evolution bonus of +3, the window becomes 9 frames (150ms). The window never activates after a wall release or external-force push.
**Example:** Creature runs off a ledge at frame 0. At frame 4, player presses jump. `frames_off = 4, coyote_frames = 6, no_jump_exit = true`. `4 <= 6 AND true = true` -- coyote jump is valid; ground jump impulse is applied. At frame 7: `7 <= 6 = false` -- coyote expired, no jump.

---

### F-5: Jump Buffer Window

`buffer_active = (frames_since_jump_pressed <= buffer_frames) AND (jump_not_consumed == true)`

`buffer_frames = 6`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| frames_since_jump_pressed | `frames_buffered` | int | 0–unbounded | Physics frames elapsed since the player last pressed jump while not on a jumpable surface |
| buffer_frames | `buffer_frames` | int | 6 | Fixed at 6 frames = 100ms at 60fps (CR-3b). Not modified by evolution or feel layer |
| jump_not_consumed | `unconsumed` | bool | true/false | True until the buffer triggers a jump. Once consumed, the buffer resets |

**Output Range:** Boolean. If the creature lands within 6 frames of the jump press, the jump fires on the landing frame. Buffer applies to both ground landings and wall contacts (triggering wall jump if applicable).
**Example:** Player presses jump 3 frames before landing. On landing frame: `frames_buffered = 3, 3 <= 6 AND true = true`. Jump executes immediately on the landing frame. If landing does not occur within 6 frames, the buffer expires and the press is discarded.

---

### F-6: Wall Jump Impulse with Fatigue

`wall_jump_vertical = base_wall_impulse * (1.0 - fatigue_rate * consecutive_count)`

`wall_jump_horizontal = base_horizontal_impulse`

Clamped: `wall_jump_vertical = max(wall_jump_vertical, floor_impulse)`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| base_wall_impulse | `base_v` | float | 260.0 px/s | Base vertical impulse for wall jump (CR-4c). Applied as negative Y (upward) |
| fatigue_rate | `fatigue_rate` | float | 0.0–0.2 | Per-jump vertical impulse reduction. Base: 0.2 (20%) per consecutive same-wall jump. Reduced by evo `wall_jump_fatigue_reduction` (CR-9): effective rate = 0.2 - wall_jump_fatigue_reduction, min 0.0 |
| consecutive_count | `n` | int | 0–unbounded | Number of consecutive wall jumps on the same wall without touching ground or a different wall. First jump: n=0 (no fatigue). Second: n=1. Third: n=2. Etc. |
| floor_impulse | `floor_v` | float | 133.0 px/s | Minimum vertical impulse (CR-4c). Fatigue cannot reduce below this value. Reached at n=3 with base fatigue rate |
| base_horizontal_impulse | `base_h` | float | 160.0 px/s | Horizontal impulse away from wall (CR-4c). Not affected by fatigue. In PRECISE context: 120 px/s (CR-4c) |

**Output Range:** Vertical: 260 px/s (n=0) down to 133 px/s (n>=3). Heights achieved: 1st jump = 260^2/(2*800) = 42.3 px. 2nd = 208^2/1600 = 27.0 px. 3rd = 166^2/1600 = 17.2 px. 4th+ = 133^2/1600 = 11.1 px. With evo fatigue reduction of 0.1 (rate becomes 0.1): 1st=42.3, 2nd=34.2, 3rd=27.0, 4th=21.3, floor at n=9.
**Example:** Third consecutive wall jump on the same wall, base fatigue, no evo: `fatigue_rate = 0.2, n = 2`. `260 * (1.0 - 0.2 * 2) = 260 * 0.6 = 156.0 px/s`. Height: `156^2 / 1600 = 15.2 px` (~1 tile). Fourth jump: `260 * (1.0 - 0.2 * 3) = 260 * 0.4 = 104.0 px/s` -- below floor of 133, so clamped to 133.0 px/s.

---

### F-7: Wall Slide Grip Degradation

`wall_slide_speed = base_slide_speed * feel_slide_mult * surface_slide_mult` (for t <= grip_duration)

`wall_slide_speed = (base_slide_speed * feel_slide_mult * surface_slide_mult) + degradation_rate * (t - grip_duration)` (for t > grip_duration)

Clamped: `wall_slide_speed = min(wall_slide_speed, terminal_velocity)`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| base_slide_speed | `base_ws` | float | 80.0 px/s | Base wall slide fall speed (CR-4a) |
| feel_slide_mult | `feel_ws` | float | 0.7–1.3 | Feel layer wall slide speed multiplier (CR-6): CALM=1.0, TENSE=0.85, PANIC=1.3, PRECISE=0.7, SHELTERED=1.0 |
| surface_slide_mult | `surf_ws` | float | 0.7–2.0 | Surface slide speed multiplier (CR-7): Moss=0.8, Dense Vine=0.9, Ice=2.0, Dry Stone=1.0 |
| t | `t` | float | 0.0–unbounded (s) | Time in seconds spent in wall slide state on the current wall |
| grip_duration | `grip_dur` | float | 2.0+ s | Base: 2.0s (CR-4a). Extended by evo `wall_slide_duration_bonus`: grip_dur = 2.0 + wall_slide_duration_bonus |
| degradation_rate | `deg_rate` | float | 50.0 px/s per second | After grip_duration, slide speed increases by 50 px/s each second (CR-4a) |
| terminal_velocity | `v_term` | float | 350.0 px/s | Maximum fall speed (CR-3). Or evo `terminal_velocity_override` if >= 0 |

**Output Range:** Initial slide speed: 44.8 px/s (80 * 0.7 PRECISE * 0.8 Moss) to 208.0 px/s (80 * 1.3 PANIC * 2.0 Ice -- though Ice is not wall-interactive by default, requiring evo override). Practical initial range on interactive surfaces: 44.8 to 104.0 px/s. After degradation onset, speed increases linearly toward terminal velocity (350 px/s).
**Example:** Wall sliding on dry stone in CALM for 3.5 seconds, no evo bonus. t=3.5, grip_dur=2.0. Initial speed: `80 * 1.0 * 1.0 = 80.0 px/s`. Degraded: `80.0 + 50.0 * (3.5 - 2.0) = 80.0 + 75.0 = 155.0 px/s`. The creature is accelerating down the wall. At t=7.4s: `80 + 50 * 5.4 = 350 px/s` -- terminal velocity reached.

---

### F-8: Landing Recovery Severity

`recovery_tier = classify(fall_duration, impact_velocity)`

Classification function:

```
if fall_duration < 0.3 AND impact_velocity < 200:
    tier = SOFT          # 0 recovery frames
elif fall_duration <= 0.6 AND impact_velocity <= 300:
    tier = MEDIUM        # 4 frames (67ms), bufferable
elif fall_duration <= 1.0 AND impact_velocity <= 350:
    tier = HARD          # 10 frames (167ms), velocity zeroed, jump buffer on frame 11
else:
    tier = STAGGER       # 18 frames (300ms), velocity zeroed, 1 HP damage, buffer from frame 12
```

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| fall_duration | `t_fall` | float | 0.0–unbounded (s) | Time spent in Falling state (positive Y velocity) before floor contact |
| impact_velocity | `v_impact` | float | 0.0–350.0 px/s | Vertical velocity at moment of floor contact. Capped at terminal velocity (350 px/s) |
| recovery_frames | `r_frames` | int | 0, 4, 10, 18 | Frames of input lockout per tier (CR-11) |
| damage | `dmg` | int | 0 or 1 | Only STAGGER tier deals damage: 1 HP (CR-11) |

**Output Range:** SOFT: 0 frames, 0 damage. MEDIUM: 4 frames, 0 damage. HARD: 10 frames, 0 damage. STAGGER: 18 frames, 1 HP damage. In PANIC context (CR-11 feel interaction): HARD becomes 7 frames, STAGGER becomes 14 frames. In PRECISE context: buffer windows open 2 frames earlier within the existing frame counts.
**Example:** Creature falls for 0.5s, reaching impact velocity of `v = 0 + 1000 * 0.5 = 500` -- but capped at 350 px/s (terminal velocity reached at t = 350/1000 = 0.35s). So t_fall = 0.5s, v_impact = 350 px/s. `0.5 <= 0.6 AND 350 > 300` -- the velocity check pushes this to HARD (both conditions in a tier must be met, and the velocity exceeds MEDIUM's ceiling). Recovery: 10 frames, no damage. Normal jump from apex (total air time ~0.7s, impact velocity = 1000 * 0.35 = 350 px/s): t_fall ~ 0.35s (descent portion), v_impact = 350. `0.35 <= 0.6 AND 350 > 300` -- HARD. However, CR-11 states "most normal jumps result in soft or medium landings" with total air time ~0.7s. The fall duration here is the descent phase only: time from apex to ground. From a 49px-high jump, descent time = sqrt(2 * 49 / 1000) = 0.313s, v_impact = 1000 * 0.313 = 313 px/s. `0.313 <= 0.6 AND 313 > 300` -- this is at the MEDIUM/HARD boundary. CR-11 specifies 300 px/s as the MEDIUM ceiling, so 313 px/s is HARD by 13 px/s. This is the designed tight margin: a full-height jump barely clips into HARD territory, giving that "stumble on full commitment" feel.

---

### F-9: Air Control Effectiveness

`air_accel = base_ground_accel * air_control_ratio * feel_accel_mult`

`air_decel = base_ground_decel * air_decel_ratio`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| base_ground_accel | `ground_a` | float | 600.0 px/s^2 | Ground acceleration (CR-2) |
| air_control_ratio | `air_pct` | float | 0.6–0.85 | Feel layer air control percentage (CR-6): CALM=0.7, TENSE=0.75, PANIC=0.6, PRECISE=0.85, SHELTERED=0.7 |
| feel_accel_mult | `feel_a` | float | 0.6–1.5 | Feel layer ground acceleration multiplier (CR-6), applied before air control ratio in the computation chain |
| air_decel_ratio | `air_d_pct` | float | 0.3 | Fixed. Air deceleration is 30% of ground deceleration (CR-3). Not modified by feel layer |
| base_ground_decel | `ground_d` | float | 900.0 px/s^2 | Ground deceleration on input release (CR-2) |

**Output Range:** Air acceleration: 216.0 px/s^2 (600 * 0.6 PANIC * 0.6 SHELTERED -- note: only one feel context active at a time, so minimum practical is 600 * 0.6 PANIC = 360 * -- wait, air_control_ratio for PANIC is 0.6, feel_accel_mult for PANIC is 1.5: `600 * 0.6 * 1.5 = 540 px/s^2`). Minimum: SHELTERED: `600 * 0.7 * 0.6 = 252 px/s^2`. Maximum: PRECISE: `600 * 0.85 * 0.9 = 459 px/s^2`. Air deceleration (constant): `900 * 0.3 = 270.0 px/s^2`.
**Example:** Airborne in CALM context: `air_accel = 600 * 0.7 * 1.0 = 420.0 px/s^2`. This matches CR-3's stated 420 px/s^2 (70% of 600). In TENSE: `600 * 0.75 * 1.25 = 562.5 px/s^2` -- sharper air redirection under threat. In PRECISE: `600 * 0.85 * 0.9 = 459.0 px/s^2` -- high control ratio compensates for lower base acceleration.

---

### F-10: Effective Swim Speed

**Surface swimming:**

`swim_speed_surface = (base_surface_speed + evo_swim_bonus) * feel_swim_mult`

**Submerged swimming:**

`swim_speed_submerged = (base_submerged_speed + evo_swim_bonus) * feel_swim_mult`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| base_surface_speed | `base_surf` | float | 70.0 px/s | Surface swim speed (CR-10) |
| base_submerged_speed | `base_sub` | float | 55.0 px/s | Submerged swim speed in all directions (CR-10) |
| evo_swim_bonus | `evo_swim` | float | 0.0–unbounded | `swim_speed_bonus` from Evolution hook table (CR-9). Default 0.0 |
| feel_swim_mult | `feel_sw` | float | 0.8–1.4 | Feel layer swim speed multiplier (CR-6): CALM=1.0, TENSE=1.2, PANIC=1.4, PRECISE=0.85, SHELTERED=0.8 |

**Output Range:** Surface: 56.0 px/s (70 * 0.8 SHELTERED, no evo) to 98.0 px/s (70 * 1.4 PANIC, no evo). With +20 evo bonus in PANIC: (70 + 20) * 1.4 = 126.0 px/s. Submerged: 44.0 px/s (55 * 0.8) to 77.0 px/s (55 * 1.4). Submerged diagonal movement is normalized to the calculated speed magnitude (CR-10).
**Example:** Surface swimming in TENSE, with gill-lungs evo granting +15 swim bonus: `(70.0 + 15.0) * 1.2 = 102.0 px/s`. Submerged in CALM, no evo: `(55.0 + 0.0) * 1.0 = 55.0 px/s`.

---

### F-11: Oxygen Depletion

`effective_oxygen_duration = base_oxygen * evo_oxygen_mult`

`oxygen_remaining = effective_oxygen_duration - time_submerged`

`damage_per_second = 1 HP/s when oxygen_remaining <= 0`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| base_oxygen | `base_O2` | float | 5.0 s | Base oxygen duration when submerged (CR-10). Does not deplete when at the water surface |
| evo_oxygen_mult | `evo_O2` | float | 1.0–3.0+ | `swim_oxygen_multiplier` from Evolution hook table (CR-9). Default 1.0. Gill-lungs mutation sets to 3.0 |
| time_submerged | `t_sub` | float | 0.0–unbounded (s) | Time spent in SWIMMING_SUBMERGED state. Resets when surfacing or exiting water |
| depletion_rate | `dep_rate` | float | 1.0 O2/s | Oxygen depletes at 1.0 per second of submersion (CR-10) |
| damage_rate | `dmg_rate` | int | 1 HP/s | Damage dealt per second when oxygen is fully depleted (CR-10) |

**Output Range:** Oxygen duration: 5.0s (no evo) to 15.0s (gill-lungs at 3.0x). While oxygen > 0, no damage. At oxygen = 0 and below, 1 HP per second continuously until the creature surfaces, exits water, or dies.
**Example:** Base creature (no evo) dives. effective_duration = 5.0 * 1.0 = 5.0s. After 3.0s submerged: `oxygen_remaining = 5.0 - 3.0 = 2.0s` remaining. After 6.0s: `oxygen_remaining = 5.0 - 6.0 = -1.0` -- creature has taken 1 HP damage (1 second past depletion). With gill-lungs (evo_O2 = 3.0): effective_duration = 5.0 * 3.0 = 15.0s. After 12.0s submerged: `oxygen_remaining = 15.0 - 12.0 = 3.0s` remaining -- still safe.

---

### F-12: External Force Application

`total_velocity = creature_velocity + external_force_vector`

`stun_triggered = external_force_magnitude > stun_threshold`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| creature_velocity | `v_creature` | Vector2 | (-350, -350) to (350, 350) px/s | The creature's own velocity from movement physics (capped at terminal velocity per axis) |
| external_force_vector | `F_ext` | Vector2 | unbounded | Additive velocity from external sources: wind, water current, knockback, explosions (CR-12d). Bypasses the feel layer. Decays at the source system's specified rate |
| external_force_magnitude | `|F_ext|` | float | 0.0–unbounded | `F_ext.length()`. The magnitude of the external force vector |
| stun_threshold | `stun_thresh` | float | 200.0 px/s | When `|F_ext|` exceeds 200 px/s, the creature enters STUNNED state (CR-12d). Stun duration is specified by the source system, not by the Player Controller |

**Output Range:** Total velocity is the sum of creature velocity and all active external forces. There is no cap on the combined result -- a strong wind can push the creature beyond its normal terminal velocity. Stun activates as a binary threshold at 200 px/s force magnitude.
**Example:** Creature running right at 120 px/s. A wind force of (-80, 0) px/s blows left. `total_velocity = (120 + (-80), 0) = (40, 0) px/s` -- creature moves right but slowly. No stun: `|-80| = 80 < 200`. Creature knocked back by a predator at (250, -100) px/s: `|F_ext| = sqrt(250^2 + 100^2) = 269.3 px/s > 200` -- STUNNED state triggered. Total velocity on that frame: `(120 + 250, 0 + (-100)) = (370, -100) px/s` -- the creature is launched right and upward.

---

### F-13: Slope Speed Modifier

`slope_speed = effective_ground_speed * slope_modifier(angle)`

```
slope_modifier(angle):
    if angle <= 45:    return 1.0                                    # full speed
    if angle <= 60:    return 0.5 (uphill) or 1.25 (downhill)       # partial
    if angle > 60:     return 0.0 (treated as wall, not traversable) # wall
```

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| effective_ground_speed | `v_ground` | float | 0.0–~200 px/s | Output of F-1 (effective ground speed after all multipliers) |
| angle | `theta` | float | 0.0–90.0 degrees | Floor angle from horizontal. Read from `get_floor_normal()` on the CharacterBody2D. 0 = flat, 90 = vertical wall |
| slope_modifier | `s_mod` | float | 0.0, 0.5, 1.0, or 1.25 | Discrete speed modifier based on angle bracket and direction (CR-12b) |
| direction | `dir` | enum | uphill/downhill | Whether the creature moves against or with the slope's gradient |

**Output Range:** 0.0 px/s (slope > 60 degrees, treated as wall) to ~250 px/s (max ground speed * 1.25 downhill). The `floor_snap_length` of 8px prevents sliding on idle (CR-12b).
**Example:** Running at 120 px/s on a 30-degree slope uphill: `120 * 1.0 = 120 px/s` -- no penalty. Running at 120 px/s on a 50-degree slope uphill: `120 * 0.5 = 60 px/s`. Running downhill on the same 50-degree slope: `120 * 1.25 = 150 px/s`. At 65 degrees: the surface is treated as a wall, ground movement stops, wall slide activates if the surface is wall-interactive.

---

### F-14: Feel Layer Interpolation

`current_mult = lerp(previous_mult, target_mult, t / transition_duration)`

Or equivalently per-frame:

`current_mult = previous_mult + (target_mult - previous_mult) * (elapsed_frames / total_transition_frames)`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| previous_mult | `m_prev` | float | 0.5–1.5 | The multiplier value from the previous feel layer context (varies per parameter, see CR-6 table) |
| target_mult | `m_target` | float | 0.5–1.5 | The multiplier value for the incoming feel layer context (varies per parameter, see CR-6 table) |
| t | `t` | float | 0.0–1.0 | Interpolation progress. 0.0 = fully previous, 1.0 = fully target |
| elapsed_frames | `f_elapsed` | int | 0 to total_transition_frames | Physics frames elapsed since the context transition began |
| total_transition_frames | `f_total` | int | 0, 10, or 20 | Transition TO PANIC: 0 frames (instant). Default transition (between non-PANIC contexts): 10 frames (167ms). Transition FROM PANIC to any context: 20 frames (333ms) (CR-6) |

**Output Range:** The interpolated multiplier smoothly transitions between two CR-6 table values. Each parameter in the feel layer table is interpolated independently. During a 10-frame transition, each frame moves 10% toward the target. During a 20-frame PANIC comedown, each frame moves 5%.
**Example:** Transition from CALM to TENSE (default 10-frame lerp). Max run speed multiplier goes from 1.0 to 1.17. At frame 4 of 10: `t = 4/10 = 0.4`. `current = 1.0 + (1.17 - 1.0) * 0.4 = 1.0 + 0.068 = 1.068`. Effective run speed at that frame: `120 * 1.068 = 128.2 px/s`. Transition FROM PANIC (1.33) to CALM (1.0), 20 frames. At frame 10: `t = 10/20 = 0.5`. `current = 1.33 + (1.0 - 1.33) * 0.5 = 1.33 - 0.165 = 1.165`. The creature is still running faster than CALM halfway through the comedown. Transition TO PANIC: instant. `current = target` on the frame the context changes. No interpolation.

---

### F-15: Surface Friction Composite

`effective_friction = max(base_friction * weather_override, evo_friction_minimum)`

When no weather override is active: `weather_override = 1.0` (identity).

When weather provides an override for the current surface type: `weather_override` replaces `base_friction` entirely (it is a full override, not a multiplier on top of the base value):

`effective_friction = max(weather_friction, evo_friction_minimum)`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| base_friction | `f_base` | float | 0.2–1.3 | Surface friction (deceleration) multiplier from the surface type table (CR-7): Ice=0.2, Metal Wet=0.4, Wet Stone=0.6, Metal Dry=0.8, Dry Stone=1.0, Fungal Mat=1.1, Root Structure=1.2, Moss=1.3 |
| weather_override | `f_weather` | float | 0.2–1.3 | Weather-applied surface friction override from Time & Weather via World/Biome (INT-8). When rain is active, exposed Dry Stone tiles receive Wet Stone friction (0.6). When frost is active, exposed surfaces receive Ice friction (0.2). If no weather effect, this is absent and base_friction is used as-is |
| evo_friction_minimum | `f_evo_min` | float | 0.0–0.8 | `surface_friction_minimum` from Evolution hook table (CR-9). Default 0.0 (no minimum). Hardened foot pads set this to 0.8 |

**Output Range:** Without evo: 0.2 (ice, or frost-covered surface) to 1.3 (moss, no weather). With hardened foot pads (min 0.8): 0.8 (ice and wet metal are clamped up to 0.8) to 1.3 (moss, unaffected by minimum since 1.3 > 0.8).
**Example:** Dry stone during rain, no evo. Rain overrides Dry Stone friction to Wet Stone value: `weather_friction = 0.6`. `effective_friction = max(0.6, 0.0) = 0.6`. Deceleration: `base_decel * 0.6`. Stop time from 120 px/s: `120 / (900 * 0.6) = 0.222s` instead of `120 / 900 = 0.133s` -- nearly twice the slide distance. Same scenario with hardened foot pads (evo_min=0.8): `effective_friction = max(0.6, 0.8) = 0.8`. The evolution clamps the rain penalty. Stop time: `120 / (900 * 0.8) = 0.167s` -- noticeably better than without the mutation. Ice with hardened foot pads: `effective_friction = max(0.2, 0.8) = 0.8`. The mutation transforms ice from nearly frictionless (0.2) to manageable (0.8).

---

### F-16: Effective Climb Speed

`climb_speed = (base_climb * surface_climb_mult) * feel_climb_mult`

Diagonal normalization: when both horizontal and vertical input are active, the velocity vector is normalized to `climb_speed` magnitude (not `climb_speed * sqrt(2)`).

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| base_climb | `base_c` | float | 50.0 px/s | Base climb speed in all directions (CR-5) |
| surface_climb_mult | `surf_c` | float | 0.7–1.1 | Surface-specific climb speed modifier (CR-5): Dry Vine=1.0, Wet Vine=0.7, Root Structure=1.1, Fungal Ladder=0.8, Rope/Cable=1.0 |
| feel_climb_mult | `feel_c` | float | 0.8–1.5 | Feel layer climb speed multiplier (CR-6): CALM=1.0, TENSE=1.3, PANIC=1.5, PRECISE=0.8, SHELTERED=0.8 |

**Output Range:** Minimum: 28.0 px/s (50 * 0.7 Wet Vine * 0.8 PRECISE). Maximum: 82.5 px/s (50 * 1.1 Root Structure * 1.5 PANIC). In CALM on dry vines: 50.0 px/s (baseline). Diagonal movement at any speed is normalized: the creature never moves faster diagonally.
**Example:** Climbing wet vines in TENSE context: `50.0 * 0.7 * 1.3 = 45.5 px/s`. Climbing root structure in PANIC: `50.0 * 1.1 * 1.5 = 82.5 px/s` -- the creature scrambles up rigid roots at maximum speed. Climbing fungal ladder in PRECISE: `50.0 * 0.8 * 0.8 = 32.0 px/s` -- deliberate, careful placement. Moving diagonally at 32 px/s: velocity vector = `(32 / sqrt(2), 32 / sqrt(2))` = `(22.6, 22.6) px/s`, magnitude = 32 px/s.

---

### F-17: Descending Velocity at Impact

`v_impact = min(gravity_descend * t_fall, terminal_velocity)`

Time to reach terminal velocity from apex (v=0):

`t_terminal = terminal_velocity / gravity_descend`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| gravity_descend | `g_down` | float | 1000.0 px/s^2 | Descending gravity (CR-3, CR-11). Higher than ascending gravity for the "heavier fall" feel |
| t_fall | `t_fall` | float | 0.0–unbounded (s) | Time spent falling (positive Y velocity) |
| terminal_velocity | `v_term` | float | 150.0–350.0 px/s | Default 350 px/s (CR-3). Overridden by evo `terminal_velocity_override` if >= 0 (CR-9). Gliding membranes set to 150 px/s |

**Output Range:** 0 px/s (just started falling) to 350 px/s (at t >= 0.35s with default terminal). With gliding membranes: capped at 150 px/s, reached at t = 150/1000 = 0.15s.
**Example:** Normal fall from ledge, no evo. At t=0.2s: `v = 1000 * 0.2 = 200 px/s`. At t=0.35s: `v = 1000 * 0.35 = 350 px/s = terminal`. At t=1.0s: `v = 350 px/s` (still terminal). With gliding membrane (terminal = 150): at t=0.2s: `v = min(200, 150) = 150 px/s` -- terminal already reached. The gliding creature falls 57% slower at terminal than the unmodified creature.

---

### F-18: Moving Platform Momentum Preservation

`jump_velocity = creature_jump_impulse + platform_velocity`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| creature_jump_impulse | `v_jump` | Vector2 | (h_velocity, -impulse) | The creature's horizontal velocity at time of jump plus the vertical jump impulse |
| platform_velocity | `v_plat` | Vector2 | unbounded | Velocity of the moving platform the creature is standing on (CR-12c). Read from the KinematicBody2D/AnimatableBody2D's velocity |

**Output Range:** Additive. A platform moving right at 50 px/s adds 50 px/s to the creature's horizontal velocity on jump. A platform moving upward at 30 px/s adds -30 to the vertical component (assisting the jump). The combined result is not capped by the creature's normal terminal velocity -- momentum from platforms is preserved fully on the launch frame, then normal physics (air control, gravity) take over.
**Example:** Creature stands on a vine platform moving right at 40 px/s. Creature jumps with horizontal velocity of 100 px/s (running right) and vertical impulse of -280 px/s. `jump_velocity = (100 + 40, -280 + 0) = (140, -280) px/s`. The creature launches with 140 px/s horizontal speed, exceeding the normal max run speed of 120 px/s -- this is intended, momentum is a reward for timing.

## Edge Cases

### EC-1. State Machine Conflicts

- **If two transitions share the same priority and both conditions are true on the same frame**: The transition listed first in the table (lower row number) wins. The table is iterated top-to-bottom per priority tier, making this deterministic. In practice, same-priority transitions already have mutually exclusive conditions (e.g., transitions 41-43 share priority 54 but differ on input magnitude). If a conflict fires in a debug build, emit a `debug_transition_conflict` signal with both transition IDs for telemetry.

- **If the creature meets conditions for JUMPING (priority 80) and CRAWLING (priority 90) on the same frame** (player presses Jump and Down simultaneously while grounded): CRAWLING wins (90 > 80). The creature enters crawl. The jump input is not consumed and falls into the jump buffer (6 frames). If the player releases Down within those 6 buffer frames and standing clearance exists, the creature stands and the buffered jump executes. If the buffer expires while still crawling, the jump is discarded.

- **If a collider reshape entry action is in progress when a higher-priority transition fires** (stun force hits during the 4-frame crawl-entry reshape): The higher-priority transition (STUNNED, priority 99) interrupts immediately. The reshape is cancelled. The collider snaps to whichever shape it was closer to at the interruption frame: >= 2 frames into reshape commits to the target collider; < 2 frames reverts to the source collider. This snap is a single-frame correction; `move_and_slide()` depenetration resolves any resulting overlap on the next frame.

- **If health reaches 0 from the same force that would trigger STUNNED** (DEAD priority 100 vs. STUNNED priority 99): DEAD wins. The stun is never entered. The force is irrelevant because the creature is dead.

- **If the creature is in LANDING_RECOVERY and a SWIMMING_SURFACE transition triggers** (landed on a platform partially in water, water detector fires during recovery frames): SWIMMING_SURFACE wins (98 > 50). Landing recovery is cancelled. The creature enters water immediately. Recovery frames are forfeited.

- **If the creature is in WALL_JUMPING and contacts a floor before the 8-frame arc expires**: Transitions 41-43 require FALLING as the source state. Since the creature is in WALL_JUMPING, those transitions do not fire. Resolution: an implicit transition exists at priority 69 -- WALL_JUMPING to IDLE/WALKING/RUNNING when `is_on_floor()` is true. The same soft/medium/hard/stagger landing logic from CR-11 applies based on vertical velocity at contact. The wall-jump input lockout timer is cleared on landing.

### EC-2. Surface Transitions

- **If the creature is running and crosses from dry stone (friction 1.0) to ice (friction 0.2) mid-stride**: The new surface's friction multiplier applies starting the frame the >50% collider overlap rule (CR-7) selects ice. There is no blend or lerp between surface frictions -- the switch is instantaneous. The creature suddenly loses most of its deceleration ability. This is intentional: ice is dangerous, and the player should feel the abrupt loss of control.

- **If the creature is running and straddles two tiles at exactly 50/50 overlap**: Per CR-7, the lower-friction surface wins. This is conservative for the player (surfaces are at their most dangerous when ambiguous) and deterministic for the programmer (no floating-point area comparison at the boundary).

- **If the creature is wall-sliding on a wall-interactive surface and slides down into a non-wall-interactive surface below** (vine-covered stone transitions to smooth metal): The moment the >50% contact rule selects the non-interactive surface, the creature detaches (transition 35: WALL_SLIDING to FALLING). No partial slide -- the creature was on the wall, now it is falling.

- **If the creature is climbing and the climbable surface ends at a horizontal boundary** (left/right edge, not top/bottom): The creature cannot move further in that direction on the climbable surface. If the adjacent tile is wall-interactive but not climbable, transition 39 fires (CLIMBING to WALL_SLIDING). If the adjacent tile is neither, the creature stays at the edge of the climbable surface -- horizontal movement is blocked in that direction, but up, down, and away-from-surface movement still work.

- **If the creature is wall-clinging and the surface below (which it would slide into if cling is released) is not cling-allowed**: This is not a conflict. Wall cling zeroes vertical velocity -- the creature is stationary. When the creature releases cling, it enters WALL_SLIDING (transition 34) and slides down. If it re-presses grab on the non-cling surface, transition 33 requires `wall_cling_allowed`, which is false. The grab input is ignored, and the creature continues wall-sliding.

- **If weather changes a surface type while the creature is standing on it** (rain turns dry stone to wet stone): The new surface properties apply on the next physics frame when the Player Controller performs its surface lookup (CR-7). No retroactive slip -- the creature just has wet stone friction from that frame forward.

### EC-3. Collider Reshaping

- **If the creature begins crawl-to-stand reshape (12x6 to 10x12 over 4 frames) and a ceiling moves downward during those frames, removing clearance**: On each frame of the reshape, the controller performs a ShapeCast with the intermediate collider dimensions. If the ShapeCast detects a collision, the reshape halts at the current frame's dimensions and the creature remains in CRAWLING. The controller re-checks every frame. When clearance returns, the reshape resumes from where it stopped (it does not restart from frame 0).

- **If the creature is mid-reshape from standing to crawling (entering crawl) and the floor disappears**: The reshape completes (the creature is committed to crawling), and the creature transitions to FALLING with the crawl collider active (transition 15). On landing, the creature stays in CRAWLING (transition 44).

- **If the creature is mid-reshape from crawl to squeeze (entering squeeze) and the passage geometry widens** (crumbling tile breaks): If the passage widens enough for the crawl collider before the squeeze reshape completes, the reshape cancels. The collider reverts to 12x6 over the remaining reshape frames (symmetric reversal). The creature stays in CRAWLING.

- **If evolution changes `squeeze_collider_dimensions` while the creature is in SQUEEZING state and the new dimensions do not fit the passage**: The evolution system MUST NOT apply collider dimension changes during SQUEEZING. The evolution `mutation_applied` signal includes the current state; if state is SQUEEZING, the evolution system defers the collider change and stores it in a `pending_collider_update` flag. The change applies on the frame the creature transitions out of SQUEEZING.

- **If evolution grows the standing collider (e.g., 10x12 to 14x16) and the creature is in a space that does not fit the new dimensions**: On receiving `mutation_applied`, the controller performs a ShapeCast with the new dimensions at the current position. If the new collider fits, apply immediately. If not, search for the nearest valid position within 8px (half a tile) using 4 cardinal raycasts. If found, teleport the creature there (micro-correction, visually imperceptible). If no valid position exists within 8px, set `pending_collider_update` and re-check every frame. The creature operates with the old collider until clearance is available.

### EC-4. Water Boundaries

- **If the creature is standing on ground and the water surface is at its waist** (partial submersion, water level between feet and head): The swimming transition fires based on the creature's detector area overlapping the water volume. The detector area is at the creature's vertical center. If the water surface is above the detector center, SWIMMING_SURFACE triggers. If below, the creature remains grounded with ground movement at 80% speed (water drag applied as a surface friction modifier by the World/Biome system, not as a state change). Shallow water does not trigger swimming.

- **If the water surface is at a different vertical level than the ground** (ground slopes into water, or a step down to water): The creature transitions to SWIMMING_SURFACE when its detector area crosses the water volume boundary. On transition, the creature's vertical position snaps to the water surface line (SWIMMING_SURFACE entry action). If ground is higher than water surface, the creature effectively steps down into water.

- **If a water volume exists inside a squeeze passage**: Squeezing overrides swimming. The creature remains in SQUEEZING with squeeze movement rules (20 px/s, forward/backward only). However, the oxygen timer runs while the squeeze collider is within the water volume. The creature takes drowning damage at 1 HP/s when oxygen hits 0, even while squeezing. Flooded squeeze passages are lethal under time pressure by design.

- **If a moving platform descends onto the creature while it is surface swimming**: The creature is pushed underwater. If `water_movement_mode` is SUBMERGED or AMPHIBIOUS, transition to SWIMMING_SUBMERGED. If `water_movement_mode` is SURFACE, the creature is trapped between the platform and the water surface -- it cannot dive. The creature takes crush damage at 2 HP/s while pinned. Lateral escape (swim left/right) is possible, but diving is not. This is a level design responsibility to avoid or a deliberate trap.

- **If the creature jumps from the water surface and lands on a one-way platform directly above**: The creature lands normally (transitions 41-43). If the creature then drops through (Down + Jump), it re-enters the water (transition 45: FALLING to SWIMMING_SURFACE).

### EC-5. Evolution Mid-State

- **If `gravity_multiplier` changes mid-jump** (gliding membrane activates, setting gravity_multiplier to 0.5): The new multiplier applies on the next physics frame. Ascending gravity drops from 800 to 400 px/s^2 and descending gravity from 1000 to 500 px/s^2. The creature floats higher than the original arc. No compensation or arc correction is applied -- the mutation's effect is immediate and physical.

- **If `terminal_velocity_override` changes mid-fall** (gliding activates at 300 px/s fall speed, setting terminal velocity to 150 px/s): The creature's speed does not snap to 150 px/s. The new terminal velocity caps future gravity acceleration. The creature decelerates toward 150 px/s at a rate equal to descending gravity * gravity_multiplier. With gliding membrane (0.5), deceleration toward 150 px/s is at 500 px/s^2, reaching the new cap in approximately 0.3 seconds. This produces a smooth braking feel.

- **If `max_run_speed_bonus` changes while the creature is running faster than the new maximum**: The creature does not instantly lose speed. The new max speed acts as a soft cap -- acceleration no longer increases speed beyond it, and deceleration applies if current speed exceeds the new max by more than 10%. Below the 10% grace zone, the creature coasts to the new max via normal deceleration. This prevents jarring speed snaps.

- **If `water_movement_mode` changes from SURFACE to SUBMERGED while in SWIMMING_SURFACE**: Per INT-3, the evolution system emits `mutation_applied`. On the next frame, the controller re-evaluates. SWIMMING_SURFACE is still valid. No forced state change -- the creature stays at the surface until the player presses Down, which now triggers transition 23.

- **If `water_movement_mode` changes from SUBMERGED to SURFACE while in SWIMMING_SUBMERGED**: Forced transition to SWIMMING_SURFACE. The creature rises to the water surface at 60 px/s (buoyancy rate doubled for forced surfacing) and enters SWIMMING_SURFACE on reaching the surface line. Oxygen timer stops on transition.

- **If `can_wall_cling` is set to false while in WALL_CLINGING**: On the next frame, the AND of surface flag and evolution hook fails. Forced transition to WALL_SLIDING (equivalent to transition 34, triggered by evolution change rather than input release).

- **If evolution changes collider dimensions while wall-sliding and the new collider no longer contacts the wall**: Wall contact check fails on the next frame. Transition 35 fires (wall surface ends). The creature enters FALLING.

### EC-6. Feel Layer Transitions

- **If threat context changes from CALM to PANIC mid-jump**: Per CR-6, transition TO PANIC is instant (0 frames). All multipliers snap to PANIC values on that frame. Mid-jump: ascending gravity becomes 800 * 1.1 = 880 px/s^2, air control drops from 70% to 60%. The jump arc tightens. Jump impulse is unaffected (already applied).

- **If threat context changes from PANIC to CALM mid-jump**: Per CR-6, transition FROM PANIC lerps over 20 frames (333ms). Each parameter lerps from PANIC to CALM values. The jump that began under PANIC ends in a gradually calmer arc. This is the intended "comedown" feel.

- **If threat context changes multiple times during a single jump arc** (CALM -> TENSE -> PANIC -> TENSE): Each transition interrupts the previous lerp. If a CALM-to-TENSE lerp is at frame 5 of 10 when PANIC triggers, the lerp is abandoned and PANIC values apply instantly (TO PANIC is always instant). When PANIC drops to TENSE, the 20-frame FROM PANIC lerp begins from PANIC values. Parameters at any frame are: if lerping, the linear interpolation between source and target at current progress; if not lerping, the current context's flat values.

- **If the creature enters a PRECISE zone while in PANIC context**: Per CR-6 priority, PANIC > PRECISE. PANIC context remains active. PRECISE modifiers do not apply. A fleeing creature does not gain precision control in a tight space. The player must lose the predator to benefit from PRECISE tuning.

- **If the creature enters a SHELTERED zone while a predator is in pursuit**: Per CR-6 priority, SHELTERED > PANIC. SHELTERED context activates immediately. The Shelter & Rest system should also signal Creature AI to break pursuit. If the AI has not yet updated its pursuit flag on the frame of shelter entry, the priority system guarantees SHELTERED wins anyway. The lerp from PANIC to SHELTERED takes 20 frames (FROM PANIC rule applies regardless of target context).

### EC-7. External Forces

- **If multiple external forces apply simultaneously** (wind + creature knockback + explosion push): All forces are summed into a single composite vector. The composite is applied as one additive velocity in `move_and_slide()`. Each force decays independently at its own rate. The composite is recalculated every frame from surviving forces. The stun threshold (200 px/s) is checked against the composite magnitude, not individual forces. Two 150 px/s forces producing a 212 px/s composite WILL trigger stun.

- **If an external force pushes the creature sideways during SQUEEZING**: Forces perpendicular to the passage axis are zeroed -- the passage walls absorb them. Forces along the passage axis are applied but capped at squeeze speed (20 px/s forward, -20 px/s reverse). The stun threshold check applies to the raw force magnitude before capping, so a 250 px/s sideways force in a squeeze passage triggers stun even though the creature does not move sideways. The creature is stunned in place within the passage.

- **If an external force of exactly 200 px/s magnitude is applied**: The stun check uses strict greater-than (`> 200`), not greater-than-or-equal. 200.0 px/s does NOT trigger stun. 200.01 px/s does.

- **If an external force pushes the creature into a wall**: `move_and_slide()` handles the collision. Velocity in the wall-normal direction is zeroed. The creature slides along the wall if the force has a parallel component. No crush damage from forces against walls -- crush damage only occurs in the platform-on-water-surface scenario (EC-4) and the moving-platform-ceiling scenario (EC-9).

- **If an external force is applied during DEAD state**: Ignored. DEAD entry action disables collision and zeroes velocity. Forces applied after death have no effect.

- **If an external force pushes the creature to a ledge edge during STUNNED**: Stun zeroes all velocity on entry. Once stunned, no further movement occurs. When the stun timer expires, transition 6 (STUNNED to FALLING) fires if `is_on_floor()` is false. If the force pushed the creature to the edge but not off, the creature remains on the ledge and recovers to IDLE.

### EC-8. One-Way Platforms

- **If the creature attempts drop-through while CRAWLING on a one-way platform** (Down is already held for crawl; player presses Jump): While crawling ON a one-way platform, Jump input triggers a drop-through instead of being blocked. The creature drops through the platform with the crawl collider active (collision disabled for 6 frames) and enters FALLING in crawl (transition 15). This is a specific exception to the "no jumping while crawling" rule -- Jump is repurposed for drop-through in this context.

- **If the creature presses Down + Jump during LANDING_RECOVERY on a one-way platform**: During hard/stagger landing recovery, no input is accepted. The input is discarded. The creature must wait for recovery to complete. The jump buffer does NOT store drop-through intentions -- it stores only jump intentions.

- **If a one-way platform sits at the water surface line**: The creature can stand on the platform. The water detector is below the creature's center while standing, so swimming does NOT trigger. Dropping through the platform enters the water (transition 45 fires on the next frame). Swimming at the surface under the platform: the creature cannot climb onto it from below via surface swimming alone. The creature must jump from the water surface (transition 22, -200 px/s impulse) to clear the platform and land on top on descent.

- **If the creature is falling through a one-way platform (6-frame drop-through active) and reaches a second one-way platform below**: The 6-frame collision disable applies to ALL one-way platforms, not just the original. The creature passes through both. If the window expires between the two platforms, the creature lands on the second normally.

- **If the 6-frame drop-through window expires while the creature's collider overlaps platform geometry** (thick platform or slow fall): Collision re-enables inside the platform. `move_and_slide()` depenetration pushes the creature downward (one-way platforms only have upward collision normals). The creature exits below the platform.

### EC-9. Moving Platforms

- **If a moving platform moves horizontally out from under the creature**: `is_on_floor()` returns false on the frame contact is lost. Coyote timer starts (6 frames). The creature retains whatever velocity it had at the moment of separation (platform velocity was already part of the creature's velocity via `move_and_slide()`). No abrupt velocity change.

- **If a moving platform pushes the creature upward into a ceiling**: The creature is sandwiched. If the gap between platform and ceiling is smaller than the creature's collider height, the creature takes crush damage at 3 HP per physics frame (180 HP/s at 60fps -- lethal within 2 frames at 10 HP base). Crush is detected when `is_on_ceiling()` and `is_on_floor()` are BOTH true on the same frame.

- **If the creature is on a moving platform and the platform enters a water volume**: The water detector triggers SWIMMING_SURFACE (priority 98). The creature leaves the platform and enters surface swimming. The platform continues underneath and is ignored.

- **If the creature jumps from a moving platform and inherited velocity exceeds max run speed**: The inherited velocity is applied as initial jump velocity without clamping. The creature may briefly exceed max run speed in the air. Air control acceleration will not increase speed further (capped at max run speed), but existing velocity is not reduced. Natural air deceleration (30% of ground deceleration) gradually brings horizontal speed toward normal range.

- **If a moving one-way platform shifts position during the 6-frame drop-through window**: The 6-frame collision disable is active. If the platform moves and re-overlaps the creature when collision re-enables, the depenetration rule from EC-8 applies (creature pushed below the platform).

### EC-10. Death and Respawn

- **If the creature dies in SQUEEZING state**: DEAD fires (priority 100). Velocity zeroed, collision disabled. The death animation plays with squeeze collider dimensions. `request_respawn` fires after 30 frames. Respawn at last shelter with IDLE state and standing collider.

- **If the creature dies mid-wall-jump**: DEAD fires. The wall-jump arc is cancelled. Velocity zeroed. The sprite remains at the death position (no falling, no floating). Death animation plays at that position. Respawn proceeds from last shelter.

- **If the creature dies in SWIMMING_SUBMERGED with gill-lung evolution**: DEAD fires. Collision disabled. Oxygen timer stops. Death animation plays at the underwater position. Respawn at last shelter with full oxygen. Evolution hooks (including gill-lung) are preserved across death.

- **If the creature takes damage during the death animation** (multiple damage sources in the same zone): Once in DEAD state, `damage(amount, type)` returns immediately with no effect. Health is not decremented below 0. No additional `damage_taken` signals are emitted. The death animation is not interrupted or restarted.

- **If health reaches 0 from stagger landing fall damage**: DEAD (priority 100) is checked before LANDING_RECOVERY (priority 55). DEAD wins. The creature enters DEAD on the frame of impact. The stagger animation does not play; the death animation plays instead.

- **If `request_respawn` fires but the last shelter has been destroyed**: The Shelter & Rest system maintains a fallback respawn point. If `last_shelter_position` is invalid, the system provides the next-nearest valid shelter or the global fallback spawn point. The Player Controller trusts whatever coordinates are provided.

- **If the creature respawns into a hazard zone** (shelter area has become hazardous): The creature respawns in IDLE at the shelter position. Hazard detection is immediate -- on the next frame, the Environmental Hazards system begins applying damage. There are no invincibility frames after respawn. If the shelter is truly lethal, the creature will die again. Shelters should not become permanently lethal; temporary hazards (flooding, toxin cloud) should clear during the respawn screen transition.

### EC-11. Coyote Time and Buffers

- **If the jump buffer is active and the creature contacts a wall before contacting the floor**: The buffer is consumed by the wall contact. A wall jump executes. The buffer does not distinguish between ground jump and wall jump -- it stores only "jump was pressed." The first valid jump surface consumes it.

- **If coyote time is active and the creature contacts a wall-interactive surface during the coyote window**: Both coyote ground-jump (priority 80) and wall-slide (priority 65) conditions are met. The coyote ground-jump wins (80 > 65). The creature executes a ground-style jump. The coyote timer is consumed.

- **If coyote time was earned from a moving platform and the platform has moved away during the window**: Coyote time grants a ground-jump at the creature's current position, not the platform's position. The creature is in open air. The jump executes from wherever the creature is when jump is pressed. This may look like a double-jump to the player (creature is clearly airborne). This is correct -- coyote time is a player-forgiveness mechanic, not a simulation-accuracy mechanic.

- **If the jump buffer expires on the exact frame the creature lands**: The buffer countdown is decremented at the start of the physics frame, before transition checks. If the buffer reaches 0 on the landing frame, the buffer has expired and is NOT consumed. The creature lands without jumping. Deterministic: decrement first, check second.

- **If coyote time is active and the creature falls into water during the window**: SWIMMING_SURFACE (priority 98) fires before jump transitions (priority 80). Coyote time is wasted. The creature enters water. The coyote timer is cleared on any non-jump state transition.

- **If the jump buffer is active and the creature enters CRAWLING** (landed, Down was held): The buffer is cleared on entering CRAWLING because crawling blocks jumping. The buffered jump does not execute.

- **If the wall-jump fatigue counter is at maximum and the creature then uses coyote time from a ledge**: Coyote time produces a ground jump, not a wall jump. The ground jump uses full ground-jump impulse (-280 px/s base). Wall-jump fatigue only affects wall jumps. Touching the ground via coyote jump resets the wall-jump fatigue counter to 0.

### EC-12. Climbing Edge Cases

- **If the creature climbs to the top of a climbable surface and there is no floor above**: Auto-mantle (transition 38) requires "floor above." Without floor, auto-mantle does not trigger. The creature reaches the top edge and stops -- upward movement is blocked. The creature can still move left, right, or down on the surface. Pressing jump executes the climb-launch (CR-5: -240 px/s vertical, 100 px/s away from surface), which may carry the creature above the surface. If there is still no floor to land on, the creature enters FALLING after the arc.

- **If a crumbling tile that is a climbable surface breaks while the creature is climbing it**: The surface no longer exists. The controller's per-frame check for "is the creature contacting a climbable surface" returns false. The creature transitions to FALLING with 0 initial velocity. The grab input remains held, so if the creature falls past another climbable surface, transition 32 (FALLING to CLIMBING with grab held) fires immediately -- the creature re-grabs.

- **If a climbable surface is converted to non-climbable by weather or hazard** (vines freeze, become ice): Same as surface destruction -- the per-frame climbable check fails. The creature transitions to FALLING. If the frozen surface is still wall-interactive (ice is NOT wall-interactive per CR-7), the creature falls freely. If evolution overrides make ice wall-interactive, the creature can wall-slide.

- **If the creature is climbing a rope/cable with sway (CR-5) and jumps at maximum sway offset**: The climb-launch impulse is applied from the creature's actual position, including the +/- 2px sway offset. Launches from ropes have slight horizontal position variance (4px total range). The impulse direction is "away from the wall the rope hangs from," not "away from the current sway position." Sway offset is cosmetic and does not affect launch vector.

- **If the creature is climbing and enters a squeeze passage** (climbable surface leads into a narrowing gap): CLIMBING does not transition to SQUEEZING. The squeeze transition (transition 10) requires CRAWLING as source. A climbing creature in a narrowing gap is blocked by collision. The creature must detach (release grab, enter FALLING or wall-slide), land, enter CRAWLING, then enter the squeeze.

- **If the creature auto-mantles (transition 38, 4-frame animation) and PANIC context triggers during the mantle**: The mantle completes regardless. It is a 4-frame committed animation (67ms). Feel layer changes apply to movement parameters but not to mantle timing. After the mantle, the creature is in IDLE on the ledge above with PANIC context active.

### EC-13. Simultaneous Inputs

- **If Jump + Down are pressed simultaneously while grounded on a normal platform**: CRAWLING (priority 90) beats JUMPING (priority 80). The creature enters crawl. The jump input enters the buffer. If Down is released within 6 frames and clearance exists, the creature stands and the buffered jump executes.

- **If Jump + Down are pressed simultaneously while on a one-way platform**: Drop-through is a special-case check that runs before the general transition table. On a one-way platform, Jump + Down triggers drop-through (6-frame collision disable, creature falls through). Neither CRAWLING nor JUMPING is entered -- drop-through consumes both inputs.

- **If Grab + Jump are pressed simultaneously while contacting a climbable wall in FALLING state**: Grab triggers CLIMBING (transition 32, priority 64). CLIMBING is entered on frame N. On frame N+1, the jump input (still in the buffer or still held) triggers transition 21 (CLIMBING to JUMPING). Net effect: the creature touches the vine for 1 frame and launches. This is a valid advanced technique (vine-bounce).

- **If Grab + Horizontal-toward-wall are pressed simultaneously while falling past a wall that is both climbable and wall-interactive**: WALL_SLIDING (transition 31, priority 65) beats CLIMBING (transition 32, priority 64). The creature enters wall-slide. To climb from wall-slide, an implicit transition exists at priority 63.5: WALL_SLIDING to CLIMBING when `grab_pressed AND surface.is_climbable`. This allows a fluid slide-to-climb transition without requiring the player to detach and re-grab.

- **If Left + Right are pressed simultaneously** (keyboard edge case; impossible on analog): The Input System provides a net `move_axis.x` of 0.0 (opposing inputs cancel). The Player Controller receives zero horizontal input. The creature decelerates to idle.

- **If Jump + Interact (Grab) are pressed simultaneously near a Narrative Discovery node while grounded**: Jump is a state transition (priority 80). Discovery interaction is handled externally by the Narrative system reading grab input. Both fire: the creature jumps, and the Narrative system activates `discovery_interaction_active` (50% max speed reduction applies during the jump). The Narrative system should restrict interaction acceptance to IDLE or WALKING states to prevent this if undesirable, but that is the Narrative system's responsibility.

### EC-14. Zero and Extreme Values

- **If health is exactly 0 but the creature is in a shelter zone**: DEAD fires unconditionally when `health <= 0` (priority 100). Being in a shelter does not prevent death. SHELTERED feel context is irrelevant to health checks. The creature dies in the shelter and respawns at that shelter's position. If a "safe at 1 HP in shelter" mechanic is desired, the Shelter & Rest system must clamp incoming damage to prevent health reaching 0 while sheltered -- the Player Controller does not implement this.

- **If maximum evolution bonuses stack to extreme values** (e.g., `max_run_speed_bonus` = 200, `gravity_multiplier` = 0.1): The Player Controller applies bonuses without clamping valid-range values. The Biological Evolution system is responsible for preventing degenerate stacking. As a safety net, the controller enforces absolute hard caps: max horizontal speed 500 px/s, max vertical speed 600 px/s (up or down), minimum `gravity_multiplier` 0.05. These caps emit a `parameter_clamped` warning signal in debug builds.

- **If effective speed is below 1 px/s** (SHELTERED * hazard slowdown * moss deceleration stacking): The creature still moves. Sub-pixel movement is accumulated as a float. `move_and_slide()` processes fractional pixel movement; the visual position updates when accumulated movement crosses a pixel boundary. At 0.5 px/s, the creature moves 1 pixel every 2 seconds. There is no minimum movement speed threshold.

- **If all feel layer multipliers and evolution modifiers produce a parameter value of 0 or negative**: Any computed parameter reaching 0 is clamped to 0.01 (effectively frozen but avoids division-by-zero in time-to-target calculations). Any parameter going negative is clamped to 0.01 and emits `parameter_clamped`. Negative speed is never allowed.

- **If oxygen reaches exactly 0.0 on the same frame the creature exits water**: The creature exits water (SWIMMING to IDLE/FALLING). The oxygen timer stops on the exit-action frame. No damage is dealt at exactly 0.0 on the exit frame -- damage applies "per second while submerged," and the creature is no longer submerged. The creature escapes with 0 oxygen, which refills passively (rate defined by Shelter & Rest / Environmental systems).

- **If `wall_jump_fatigue_reduction` from evolution equals or exceeds the base 20% fatigue per jump**: Per-jump fatigue is clamped to a minimum of 1% (0.01 multiplier reduction). Fatigue can be nearly eliminated but never zeroed. With 1% fatigue: first jump -260, second -257.4, third -254.8 -- effectively unlimited wall-jump climbing, but technically degrading. This is an intentional power ceiling for late-game evolution builds.

- **If a landing impact exceeds 350 px/s** (due to evolution increasing terminal velocity mid-fall): Any landing above 350 px/s uses the stagger landing tier (18-frame recovery, 1 HP damage). Fall velocities beyond base terminal velocity do not create a new damage tier -- stagger is the maximum landing penalty from the Player Controller. Additional extreme-fall damage belongs in the Environmental Hazards system if desired.

- **If `coyote_time_bonus_frames` from evolution makes coyote time exceed 18 frames** (300ms): Coyote time is hard-capped at 18 frames (300ms) regardless of bonuses. This cap is enforced by the Player Controller. 300ms is already extremely generous (most platformers use 80-120ms). Evolution can extend coyote time from 100ms to 300ms but no further.

## Dependencies

### Hard Dependencies (system cannot function without these)

| System | Direction | Nature | Interface Summary |
|--------|-----------|--------|-------------------|
| **Input System** | Upstream (Input → Controller) | Hard | Provides `move_axis`, `jump_pressed/held/released`, `grab_pressed/held/released`, `down_pressed/held`. Controller never reads raw device input. |
| **World/Biome System** | Upstream (World → Controller) | Hard | Provides `surface_type` per tile, `zone_flags`, `threat_context` (aggregated), `biome_gravity_modifier`, `weather_surface_modifier`, `wind_force`. Controller provides `global_position` back. World is the single upstream source for all environmental data. |

### Soft Dependencies (enhanced by, but functional without)

| System | Direction | Nature | Interface Summary |
|--------|-----------|--------|-------------------|
| **Biological Evolution** | Bidirectional | Soft | Evolution writes to 18 hooks in CR-9 (speed bonuses, collider changes, surface overrides, hazard resistances). Controller provides `current_state`, `time_in_state`, `cumulative_biome_exposure`, `damage_sources`. Without evolution, all hooks use defaults — controller works at base values. |
| **Shelter & Rest** | Bidirectional | Soft | Provides `is_in_shelter` (drives SHELTERED context), `last_shelter_position`, `respawn_state`. Controller emits `request_respawn` on death. Without shelters, respawn falls back to world origin and SHELTERED context never activates. |
| **Environmental Hazards** | Downstream reads, upstream damage | Soft | Hazards call `damage(amount, type)` on controller and apply forces via CR-12d. Controller provides `global_position`, `hazard_resistances`. Without hazards, no damage from environment — controller still works. |
| **Narrative Discovery** | Bidirectional | Soft | Provides `near_discovery_node` and `discovery_interaction_active` (50% speed reduction). Controller provides position data. Without narrative system, no speed reduction — controller works at full speed everywhere. |

### Downstream Consumers (read-only — these systems depend on Player Controller)

| System | What It Reads | Contract |
|--------|---------------|----------|
| **Camera System** | `global_position`, `velocity`, `current_state`, `facing_direction`, `threat_context` | Read-only. Camera never modifies controller state. |
| **Diegetic UI** | `health_current/max`, `oxygen_current`, `current_state`, `threat_context`, `velocity`, `is_in_shelter`, `evolution_hooks` | Read-only. UI never modifies controller state. |
| **Player Visual** | `current_state`, `velocity`, `facing_direction`, `threat_context`, `is_on_floor`, `wall_direction`, `is_crawling`, `is_squeezing`, `evolution_hooks` | Read-only. Visual system owns AnimatedSprite2D, controller owns physics body. |
| **Creature AI** | `global_position`, `velocity`, `current_state` | Read indirectly. AI uses player data for detection/pursuit decisions. No direct coupling — physical interactions via external forces. |
| **Time & Weather** | None directly | All weather effects reach controller through World/Biome aggregation. No direct dependency. |

### Dependency Notes

- **World/Biome as aggregation layer**: The Player Controller has only TWO hard upstream dependencies (Input, World/Biome). Creature AI, Time & Weather, and Environmental Hazards all feed into World/Biome, which aggregates their effects into `threat_context`, `weather_surface_modifier`, and `wind_force`. This keeps the controller's dependency surface minimal.
- **No circular dependencies**: All data flows are unidirectional or have clearly separated read/write contracts. The Evolution bidirectional flow uses a shared resource (hook table) with strict ownership — Evolution writes, Controller reads.
- **Provisional interface**: Input System has no GDD (Foundation layer — architecture spec only). The input contract defined in INT-1 is provisional and will be finalized during `/create-architecture`.

## Tuning Knobs

All values below must be stored in an external data resource (`player_feel.tres` or equivalent). None may be hardcoded. Knobs are grouped by system area. Each knob lists its safe range, what breaks at extremes, and which other knobs it interacts with.

### Ground Movement

| Knob | Default | Safe Range | Too Low | Too High | Interacts With |
|------|---------|------------|---------|----------|----------------|
| `base_walk_speed` | 60 px/s | 30–90 | Creature feels stuck, frustrating in safe zones | Walk/run distinction collapses — no speed gradient | `base_run_speed`, feel layer multipliers |
| `base_run_speed` | 120 px/s | 80–180 | Traversal drags, pursuit escapes feel impossible | Screen-crossing too fast at 480px viewport, level design breaks | `base_walk_speed`, feel layer multipliers, surface friction |
| `ground_acceleration` | 600 px/s² | 300–1200 | Movement feels sluggish, input lag perception | Instant-feeling, loses "body with mass" identity | Feel layer multipliers, surface accel modifiers |
| `ground_deceleration` | 900 px/s² | 400–1500 | Ice-skating everywhere, imprecise stops | Instant stops, loses momentum feel | Surface friction multipliers |
| `turnaround_deceleration` | 1200 px/s² | 600–2000 | Slow turnarounds feel unresponsive | Turnaround too snappy, loses skid animation | `turnaround_skid_frames` |
| `turnaround_skid_frames` | 4 | 2–8 | Turnaround too snappy, no weight | Turnaround commits feel punishing | `turnaround_deceleration` |

### Jumping

| Knob | Default | Safe Range | Too Low | Too High | Interacts With |
|------|---------|------------|---------|----------|----------------|
| `jump_impulse` | 280 px/s | 200–400 | Can't clear 2-tile gaps, level design breaks | Creature flies, loses grounded feel | `ascending_gravity`, `descending_gravity` |
| `ascending_gravity` | 800 px/s² | 500–1200 | Floaty jumps, loses commitment | Stubby jumps, can't reach intended heights | `jump_impulse`, `descending_gravity` |
| `descending_gravity` | 1000 px/s² | 600–1400 | Floaty descent, breaks landing feel | Slams down, minimal air time for correction | `terminal_velocity`, landing recovery tiers |
| `terminal_velocity` | 350 px/s | 200–500 | Falls feel slow-motion | Extreme fall damage too frequent | Landing recovery thresholds |
| `jump_cut_multiplier` | 0.4 | 0.2–0.7 | Short hops nearly full height (no expression) | Short hops barely leave ground (unusable) | `jump_impulse` |
| `coyote_time_frames` | 6 | 3–12 | Tight platforming frustrating, miss-rate spikes | Exploitable for extra distance, feels floaty | Evolution bonus cap (18 max) |
| `jump_buffer_frames` | 6 | 4–12 | Precision timing too demanding | Delayed jumps feel laggy | — |

### Wall Interaction

| Knob | Default | Safe Range | Too Low | Too High | Interacts With |
|------|---------|------------|---------|----------|----------------|
| `wall_slide_speed` | 80 px/s | 40–150 | Wall feels sticky, descent too slow | Wall barely slows fall, hard to read walls | Feel layer multipliers |
| `wall_slide_grip_duration` | 2.0s | 1.0–4.0 | Can't rest on walls, punishes observation | Infinite wall camping trivializes vertical traversal | `wall_slide_degrade_rate` |
| `wall_slide_degrade_rate` | 50 px/s/s | 20–100 | Degradation too gentle, nearly infinite grip | Falls off wall quickly after grip expires | `wall_slide_grip_duration` |
| `wall_jump_vertical_impulse` | 260 px/s | 180–340 | Can't gain height on walls | Wall-jump climbing too easy | `wall_jump_fatigue_percent` |
| `wall_jump_horizontal_impulse` | 160 px/s | 80–240 | Wall jumps go mostly vertical, hard to gap | Wall jumps launch too far, loses precision | Input lockout frames |
| `wall_jump_input_lockout_frames` | 8 | 4–14 | Can redirect instantly, trivializes wall sequences | Committed too long, frustrating in tight spaces | Feel layer modifiers |
| `wall_jump_fatigue_percent` | 0.20 | 0.05–0.35 | Nearly infinite same-wall climbing | Third wall jump barely moves, punishes exploration | Evolution reduction hook |

### Climbing

| Knob | Default | Safe Range | Too Low | Too High | Interacts With |
|------|---------|------------|---------|----------|----------------|
| `climb_speed` | 50 px/s | 25–80 | Climbing feels tedious | Climbing trivializes vertical traversal | Surface type multipliers, feel layer |
| `climb_launch_vertical` | 240 px/s | 160–320 | Launch from climb is weak, feels like falling off | Launch too powerful, climbing becomes a jump platform | — |
| `climb_launch_horizontal` | 100 px/s | 50–160 | Can barely push away from wall on launch | Launch throws creature too far from surface | — |

### Swimming

| Knob | Default | Safe Range | Too Low | Too High | Interacts With |
|------|---------|------------|---------|----------|----------------|
| `surface_swim_speed` | 70 px/s | 40–110 | Water traversal painfully slow | Water too easy to cross, loses threat | Feel layer, evolution bonus |
| `submerged_swim_speed` | 55 px/s | 30–90 | Underwater feels trapped | Underwater as fast as land, no medium difference | Feel layer, evolution bonus |
| `base_oxygen_duration` | 5.0s | 2.0–10.0 | Submersion always dangerous, limits exploration | Oxygen rarely matters, removes tension | Evolution multiplier (up to 3.0x) |
| `oxygen_damage_rate` | 1 HP/s | 0.5–3.0 | Drowning too slow, no urgency | Instant death feel, frustrating | `base_oxygen_duration` |
| `water_jump_impulse` | 200 px/s | 140–280 | Hard to exit water, feels trapped | Water jump as strong as ground jump, no medium difference | — |

### Crawling / Squeezing

| Knob | Default | Safe Range | Too Low | Too High | Interacts With |
|------|---------|------------|---------|----------|----------------|
| `crawl_speed` | 40 px/s | 20–60 | Crawl sections drag, player avoids them | Crawl nearly as fast as walk, loses vulnerability | — |
| `squeeze_speed` | 20 px/s | 10–35 | Squeeze passages feel punishing | Squeeze too quick, no tension | — |
| `collider_reshape_frames` | 4 (crawl), 6 (squeeze) | 2–10 | Instant shape change, loses physical feel | Transition takes too long, frustrating under pressure | — |

### Landing Recovery

| Knob | Default | Safe Range | Too Low | Too High | Interacts With |
|------|---------|------------|---------|----------|----------------|
| `medium_landing_frames` | 4 | 2–8 | No visual feedback on medium falls | Medium falls feel punishing | — |
| `hard_landing_frames` | 10 | 6–16 | Hard falls too forgiving | Hard falls too punishing, discourages exploration | PANIC modifier |
| `stagger_landing_frames` | 18 | 12–24 | Extreme falls too forgiving | Stagger feels like a freeze, frustrating | PANIC modifier, stagger damage |
| `stagger_damage` | 1 HP | 0–3 | No consequence for extreme falls | Falls become deadly, players avoid verticality | — |
| `fall_duration_thresholds` | 0.3/0.6/1.0s | ±0.15s each | Recovery tiers activate too early | Recovery tiers activate too late | Gravity values, terminal velocity |

### Feel Layer

| Knob | Default | Safe Range | Too Low | Too High | Interacts With |
|------|---------|------------|---------|----------|----------------|
| `feel_lerp_default_frames` | 10 | 4–20 | Context transitions feel abrupt | Transitions too slow, danger doesn't register | `feel_lerp_from_panic_frames` |
| `feel_lerp_from_panic_frames` | 20 | 10–30 | Panic exits too abruptly | Panic hangover lasts too long | — |
| All feel layer multiplier values | See CR-6 table | ±20% of defaults | Context differences imperceptible | Context feels like different control schemes | Each other — the entire multiplier table is a linked set |

### Accessibility

| Knob | Default | Safe Range | Too Low | Too High | Interacts With |
|------|---------|------------|---------|----------|----------------|
| `ledge_correction_pixels` | 2 | 0–4 | Frequent frustrating near-misses | Creature teleports noticeably, breaks spatial trust | Collider dimensions |
| `extended_buffer_frames` | 12 | 8–16 | Extended mode barely different from standard | Inputs feel delayed | — |
| `extended_coyote_frames` | 10 | 8–14 | Extended mode barely different from standard | Exploitable for extra distance | — |
| `auto_grab_window_pixels` | 3 (standard), 4 (assist) | 2–6 | Grabs require pixel precision | Creature grabs surfaces from too far away | — |

### External Forces

| Knob | Default | Safe Range | Too Low | Too High | Interacts With |
|------|---------|------------|---------|----------|----------------|
| `stun_threshold` | 200 px/s | 100–400 | Frequent stuns from minor knockback | Only extreme forces stun, reduces creature interactions | Stun duration (source-defined) |

### Hard Caps (not designer-tunable — safety clamps)

| Cap | Value | Rationale |
|-----|-------|-----------|
| `max_horizontal_speed` | 500 px/s | Prevents screen-crossing in <1s. |
| `max_vertical_speed` | 600 px/s | Prevents collision tunneling at 60fps. |
| `min_gravity_multiplier` | 0.05 | Prevents zero-gravity exploits from evolution stacking. |
| `max_coyote_frames` | 18 | 300ms is already extreme — prevents platform exploitation. |
| `min_collider_dimension` | 4px | Prevents sub-pixel colliders from evolution bugs. |

## Visual/Audio Requirements

The Player Controller does not own visual or audio playback -- the Player Visual System (INT-6) and Audio systems consume controller state and drive presentation. This section specifies what those downstream systems must produce in response to controller-emitted data. Everything here is the controller's **output contract**: what visual/audio behaviors the controller's state machine mandates.

**Governing principle from the art bible:** The creature animates like a documentary subject, not a game character. All visual feedback is diegetic -- communicated through the creature's body and its physical interaction with surfaces. No HUD elements, no floating indicators, no post-processing mood overlays.

---

### VAR-1. Animation Requirements

#### VAR-1.1 Animation Architecture

**Sprite resolution tiers** (from art bible section 5.1.4):
- **16x16 base**: 3-4 frame cycles, 3-4 body colors, silhouette-only legibility
- **20x20 to 24x24 mid-evolution**: 4-6 frame cycles, articulated limbs, dorsal ridge gains complexity
- **28x28 to 32x32 late evolution**: 6-8 frame cycles, full creature detail, secondary animation elements

All frame counts below are specified for the **16x16 base sprite**. Mid-evolution sprites add 1-2 frames per cycle. Late-evolution sprites add 2-4 frames per cycle. The Player Visual system reads `evolution_hooks` to select the appropriate sprite set and frame count tier.

**Playback rate**: All animations run at 10 fps (6 frames per game tick at 60fps) unless a feel context modifier or health degradation alters timing. This gives a slightly twitchy, small-animal cadence at 16x16 that slows to deliberate weight at 32x32.

**Facing**: All locomotion animations exist in two horizontal directions (left/right). The Player Visual system reads `facing_direction` from the controller. Sprites are NOT mirrored programmatically -- left-facing and right-facing are separate assets to preserve the dorsal ridge's micro-asymmetry (art bible section 3.1.1, rule 3).

---

#### VAR-1.2 Per-State Animation Specifications

Each state below lists the base animation, then how the three feel-relevant contexts (CALM, TENSE, PANIC) modify it. PRECISE and SHELTERED are included where they differ meaningfully from CALM. States that cannot occur in certain contexts (e.g., SQUEEZING is always PRECISE) note the constraint.

---

**STATE: IDLE**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 8 frames (looping) |
| Cycle duration | 0.8s at 10fps |
| Key poses | F1: Neutral stand, weight centered, dorsal ridge at rest height. F3: Weight shifts to leading limbs, head drops 1px (investigating ground). F5: Weight shifts back, head raises 1px. F7: Ridge settles with 1px micro-movement. F2/F4/F6/F8: Interpolation frames. |
| Silhouette requirement | Bottom-heavy mass, 8-10px wide, 10-12px tall. Ridge breaks top contour in every frame. 1px negative space between lowest limb pixels and ground tile. |
| Dorsal ridge | Resting height. Shimmer pulse every 90 frames (1.5s) at Intact health. |

**Feel context modifiers for IDLE:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Full 8-frame cycle. Context-sensitive: in organic biomes, F3 includes sniffing motion (head tilts down 1px further). In ruin biomes, F3 becomes a quick head-turn scan (head rotates, body holds still). |
| TENSE | Cycle compresses to 4 frames. Weight shifts forward (center of gravity moves 1px toward facing direction). Ridge raises 1px in all frames. Head holds facing-forward orientation -- no investigation behaviors. Body is 1px taller (ears/sensory structures erect). Frame timing is uneven: F1 and F3 hold 50% longer than F2 and F4 (stillness punctuated by quick adjustments). |
| PANIC | Should not occur (PANIC triggers locomotion, not idle). If the creature somehow idles in PANIC (cornered, no exit), cycle is 2 frames: compressed body, ridge fully erect, rapid 1px lateral weight shift (the creature is vibrating in place). |
| PRECISE | 6-frame cycle, slower timing (12fps effective). Careful, deliberate weight shifts. No head investigation -- full attention forward. |
| SHELTERED | 12-16 frame cycle (art bible section 2.6). Creature settles into lowest, widest posture. Limbs extend. Ridge rests flat. Breathing cycle (1px vertical expansion/contraction) becomes dominant motion. Head tucks. Body relaxes to horizontal ground-hugging orientation. |

---

**STATE: WALKING**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 3 frames (looping) |
| Cycle duration | 0.3s at 10fps |
| Key poses | F1: Contact -- leading limb touches ground, body at lowest point. F2: Pass -- body lifts 1px as weight transfers, trailing limb swings forward. F3: Reach -- leading limb fully extended forward, body at highest point, ridge flattens slightly. |
| Silhouette requirement | Quick, low, efficient. Weight stays close to ground. Visible contact-point shifting between frames. At 16x16, limbs are 1px contact points -- gait is conveyed by body-mass oscillation. |
| Speed-to-animation sync | Walk animation plays when `abs(velocity.x)` is between 15 and 60 px/s. Playback rate scales linearly with speed (0.25x at 15px/s, 1.0x at 60px/s). |

**Feel context modifiers for WALKING:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Unhurried, efficient scurry. |
| TENSE | Stride shortens (body oscillation reduced to 0px vertical). Movement becomes a creep -- body stays at constant low height. Head leads body by 1px (scanning forward). Ridge raised 1px. Frame timing: F1 holds 1 extra tick (deliberate foot placement). |
| PANIC | Walk speed rarely occurs in PANIC (acceleration is fast). If present, animation is the same 3 frames but at 2x playback rate. Body is compressed 1px vertically. |
| SHELTERED | Slowest walk. Body is at maximum relaxed width. Ridge flat. 4-frame cycle (added frame: F4 is a slight pause, creature assessing comfort of the space). Playback at 0.7x. |

---

**STATE: RUNNING**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 4 frames (looping) |
| Cycle duration | 0.4s at 10fps |
| Key poses | F1: Drive -- rear limbs push, body tilts forward 1px from horizontal. F2: Stretch -- body elongates horizontally, dorsal ridge flattens (streamlining, per art bible). F3: Gather -- body compresses, limbs come under center mass. F4: Launch -- body extends, brief 0px ground contact (airborne micro-moment). |
| Silhouette requirement | Body extends horizontally during run. Ridge flattens from resting height. Head leads body. At 16x16, the body mass shifts forward 1-2px relative to idle positioning. |
| Speed-to-animation sync | Run animation plays when `abs(velocity.x)` exceeds 60 px/s. Playback rate: 1.0x at 60px/s, scaling to 1.5x at max speed. |

**Feel context modifiers for RUNNING:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Efficient, purposeful. |
| TENSE | Body stays lower (1px more compressed). Stride lengthens (F2 stretch is 1px longer). Ridge stays raised despite streamlining (alert even at speed). Eyes forward -- no lateral head movement in any frame. 4 frames, playback at 1.2x base rate. |
| PANIC | 6 frames (art bible section 2.3: run cycle adds 2 frames for desperate speed). F5: stumble-recovery frame (body drops 1px, catches). F6: desperate push (maximum horizontal extension). Head tucked. Ridge fully flat -- survival over display. Playback at 1.5x base rate. Body trails a single movement pixel (shed bioluminescent residue per art bible section 9 reference: diegetic afterimage). |
| PRECISE | Run speed is capped lower (90 px/s), so run animation plays but at reduced rate. Body stays more upright. Each footfall is deliberate -- F1 and F3 hold 1 extra tick. Ridge at mid-height. |
| SHELTERED | Max speed is 50% of base, so running is a gentle lope. 3-frame cycle. Relaxed body. Ridge flat by choice, not by streamlining. |

---

**STATE: JUMPING**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 3 frames (non-looping sequence) |
| Duration | Plays once over the ascending portion of the jump arc (~0.35s to apex) |
| Key poses | F1: Anticipation -- body compresses 1px vertically, limbs gather under mass (plays for 2 game frames before launch, synced to the physics frame where jump impulse applies). F2: Launch -- body extends vertically, limbs trail below, ridge raises. F3: Apex approach -- body reaches maximum vertical extension, limbs begin to spread (preparing for air control). |
| Silhouette requirement | Vertical extension. Body becomes taller than wide during jump. Ridge is at maximum height. Clear gap between creature and ground surface. |
| Transition | On reaching apex (velocity.y crosses zero), blends to FALLING F1 over 1 frame. |

**Feel context modifiers for JUMPING:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Clean, efficient launch. |
| TENSE | F1 anticipation is 1 game frame instead of 2 (faster commitment). Body extends more aggressively in F2 -- 1px taller at peak extension. Ridge sharply erect. |
| PANIC | No F1 anticipation frame (impulse is instant at PANIC transition speed). F2 is explosive -- body extends to maximum, limbs splay. F3 has head oriented toward escape direction (not upward). The creature is jumping AT something (safety), not just upward. |
| PRECISE | Full 2-frame anticipation. F2 is controlled -- less vertical extension. F3 holds longer (slower apex approach per reduced ascending gravity). Body stays compact. |

---

**STATE: FALLING**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 2 frames (looping) |
| Cycle duration | 0.2s at 10fps |
| Key poses | F1: Spread -- limbs extend outward and downward, body widens. Weight distributes for air resistance. Ridge at rest. F2: Tuck -- slight contraction, limbs pull 1px closer to body. Ridge flattens. |
| Silhouette requirement | Wider than tall (contrast with jumping's taller-than-wide). Dispersed limb positioning communicates lack of surface contact. |
| Velocity-driven modification | At terminal velocity (350 px/s), F1 and F2 play at 2x rate and body is maximally compressed (creature bracing for impact). Below 200 px/s, animation plays at 0.5x rate (gentle drift). |

**Feel context modifiers for FALLING:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Controlled fall posture. |
| TENSE | Limbs extend further in F1 (maximizing air drag instinctively). Body is more rigid -- less oscillation between F1 and F2. Head orients downward (watching the landing). |
| PANIC | Single frame: fully compressed body, limbs tight, ridge flat, head tucked. The creature is a falling projectile, not a gliding form. No looping -- holds this pose until impact. |
| PRECISE | Full 2-frame loop. Limbs actively adjust position between frames (air control is higher in PRECISE). Body has more horizontal range of motion in the animation. |

---

**STATE: WALL_SLIDING**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 3 frames (looping) |
| Cycle duration | 0.3s at 10fps |
| Key poses | F1: Grip -- limbs press against wall surface, body compressed against wall (2-3px of sprite overlaps wall tile per collider-sprite offset). Ridge flattened against wall. F2: Shift -- body drops 1px, limbs re-grip (visible grip-shift-grip pattern per art bible). F3: Settle -- body at lowest point of grip cycle before F1 re-grips higher. |
| Silhouette requirement | Body pressed flat against wall surface. Narrower profile than any ground state. Head oriented downward or toward open space. Visual overlap with wall surface (sprite extends into wall tile by 2-3px). |
| Grip degradation visual | After 2.0s of wall sliding (CR-4a), F2 and F3 add 1px of downward slip per frame. Limbs lose firm contact -- grip pixels become intermittent. This telegraphs the impending grip loss. |

**Feel context modifiers for WALL_SLIDING:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Controlled descent. |
| TENSE | Body is more rigid against wall. Head actively scans away from wall (looking for threats). Ridge raised. Frame timing accelerates slightly (0.85x multiplier on slide speed means the creature is clinging harder). |
| PANIC | Slide speed is 1.3x (faster descent). Animation plays at 1.5x rate. Body is less compressed against wall -- 1px gap forms between body and surface (the creature is barely holding on). Limbs scrabble rather than grip. |
| PRECISE | Slowest slide (0.7x speed). Body maximally compressed against wall. Every frame shows deliberate grip placement. Ridge flat. Head looks down toward the landing target. |

---

**STATE: WALL_CLINGING**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 4 frames (looping, very slow) |
| Cycle duration | 1.2s at ~3.3fps |
| Key poses | F1-F2: Hold -- body static against wall, 1px breathing cycle (expansion/contraction of body mass). F3: Scan -- head rotates 1px away from wall (checking surroundings). F4: Resettle -- body adjusts grip, 1px position shift. |
| Silhouette requirement | Identical to wall slide F1 but static. Maximum compression against wall. The creature is a feature on the wall surface. |

**Feel context modifiers for WALL_CLINGING:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Patient observation posture. |
| TENSE | Breathing cycle doubles in rate. Head in F3 scans more quickly (1 game frame instead of 3). Body is 1px more tense (shoulders/mass raised). Ridge erect. No resettle in F4 -- the creature holds perfectly still except for the breathing and scanning. |
| PANIC | The creature should not cling in PANIC (wall-cling is a deliberate stationary choice). If forced: single-frame hold, body trembling (1px jitter per game frame on alternating axis), ridge erect and flickering. |

---

**STATE: WALL_JUMPING**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 3 frames (non-looping sequence) |
| Duration | Plays during the 8-frame (133ms) input lockout period |
| Key poses | F1: Push -- body coils away from wall, limbs brace against surface. Ridge snaps erect. F2: Launch -- body extends away from wall at ~45 degree angle. Limbs trail behind. Body is at maximum horizontal extension. F3: Redirect -- body begins to orient toward the apex of the arc, limbs tuck for air control. |
| Silhouette requirement | Strong diagonal line of action from wall-contact to launch direction. The push-off is visually committed -- the body reads as a spring releasing. |

**Feel context modifiers for WALL_JUMPING:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Clean, controlled push. |
| TENSE | F1 is 1 game frame shorter (5-frame lockout). Push-off is more explosive -- body extends 1px further in F2. |
| PANIC | F1 barely exists (4-frame lockout). The wall jump is desperate -- body doesn't coil, it flings. F2 has maximum extension. F3 is already scanning for the next surface. |
| PRECISE | F1 holds 1 extra frame (10-frame lockout). Push direction is more vertical (tighter to wall). Body stays compact throughout. |

---

**STATE: CLIMBING**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 4 frames (looping) |
| Cycle duration | 0.4s at 10fps |
| Key poses | F1: Grip -- leading limbs reach to next hold. Body pressed to surface. F2: Pull -- body lifts 1px, weight transfers to leading limbs. F3: Shift -- trailing limbs release and reposition. 1px gap appears between body and surface. F4: Contact -- trailing limbs grip, body settles against surface. Grip-shift-grip-shift pattern (per art bible). |
| Silhouette requirement | Limbs splayed wide, body pressed close to surface. Movement is halting -- each frame holds long enough to read as a distinct position. At 16x16, limbs are 1px extensions from body mass at wider angles than any ground state. |
| Directional variants | Animation plays the same cycle for all 8 climb directions. The Player Visual system rotates the head orientation (1-2px head position shift) to match `velocity` direction. |

**Feel context modifiers for CLIMBING:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Careful, methodical. |
| TENSE | Speed is 1.3x. Frame timing is uneven -- F1 and F4 (contact frames) are shorter, F2 and F3 (movement frames) are longer. The creature grips and moves quickly but lingers during the exposed transition moments. Ridge raised. |
| PANIC | Speed is 1.5x. Animation becomes 3 frames (F3 and F4 merge -- the creature doesn't settle, it scrambles). Limb placement is less symmetrical (frantic, imprecise gripping). |
| PRECISE | Speed is 0.8x. Full 4-frame cycle with each frame holding 50% longer. Every grip is deliberate. Head tracks movement direction precisely. |

---

**STATE: CRAWLING**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 3 frames (looping) |
| Cycle duration | 0.3s at 10fps |
| Key poses | F1: Low push -- body flattened to 6px tall (matching crawl collider 12x6). Limbs splay laterally. Leading limbs pull. F2: Drag -- body inches forward, trailing limbs push. Ridge fully compressed against ceiling. F3: Reset -- micro-pause as the creature re-gathers before the next push. |
| Silhouette requirement | Radically different from standing states. Body is twice as wide as tall. The creature fills the horizontal space of the passage. Ridge must still break the top contour even when compressed (it flattens but does not disappear). |

**Feel context modifiers for CRAWLING:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Careful low movement. |
| TENSE | Faster playback (1.3x). Body lower than necessary (creature pressing itself as flat as possible). Head very low. Ridge pressed completely flat -- bare 1px contour break. |
| PANIC | Cannot move faster while crawling (physics limits crawl speed). Animation plays at TENSE rate but adds a visible desperation: F2 drag is longer, F3 pause is eliminated (continuous dragging motion). |
| PRECISE | Default context for crawling in tight spaces. Slow, deliberate push-drag. Each frame holds 1 extra game tick. |

---

**STATE: SQUEEZING**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 2 frames (looping, very slow) |
| Cycle duration | 0.4s at 5fps |
| Key poses | F1: Compress -- body at minimum dimensions (8x4 collider), every pixel packed tight. Only forward-facing limb pixels visible. Ridge is a single unbroken line of 1-2 pixels. F2: Inch -- body shifts 1px forward. Slight body deformation (the creature visually distorts as it presses through the gap). |
| Silhouette requirement | Minimal. The creature is barely recognizable as the player -- it is a compressed mass. Ridge identity line is the only identifier. The visual must communicate physical effort and vulnerability. |
| Context lock | Always PRECISE regardless of external threat context. Single animation variant. |

---

**STATE: SWIMMING_SURFACE**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 4 frames (looping) |
| Cycle duration | 0.4s at 10fps |
| Key poses | F1: Float -- body at water surface line, top 30% above water (per CR-10). Limbs paddle below surface (implied by body oscillation). F2: Stroke -- body shifts 1px forward, dips 1px into water. F3: Rise -- body bobs up from stroke. Head clears surface. F4: Glide -- body at maximum surface height, brief stillness before next stroke. |
| Silhouette requirement | Only the top 30% of the sprite is fully rendered above the waterline. Below the waterline, the sprite is rendered at 50% opacity or with a water overlay (blue-tinted, per biome palette). Ridge visible above water in all frames. |
| Surface interaction | Water surface tiles deform (1px wave displacement) in sync with F2 stroke. See VAR-4 for splash VFX. |

**Feel context modifiers for SWIMMING_SURFACE:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Leisurely paddle. |
| TENSE | Stroke rate increases (3-frame cycle, F4 glide removed). Body sits 1px lower in water (lower profile). Head actively scans. |
| PANIC | Fastest swim rate (1.4x speed). Body thrashes -- F2 dip is 2px instead of 1px. Splashing increases (see VAR-4). Head facing forward only. |
| SHELTERED | Barely moving. 6-frame cycle with extended glide. Body relaxed in water. Head may turn to observe surroundings. |

---

**STATE: SWIMMING_SUBMERGED**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 4 frames (looping) |
| Cycle duration | 0.4s at 10fps |
| Key poses | F1: Undulate start -- body curves into a C-shape, tail leads motion (dorsal ridge becomes keel, per art bible). F2: Drive -- body straightens and extends, propulsive stroke. F3: Undulate mid -- body curves opposite direction. F4: Glide -- body straight, limbs tucked or paddling depending on evolution path. |
| Silhouette requirement | Fully submerged sprite is rendered with water overlay (full sprite at 80% opacity with blue-shift per biome water palette). Body is elongated and horizontal. Limb configuration changes with evolution (tuck vs. paddle read from `evolution_hooks`). |
| Oxygen visual | Bubble particles emit from the creature's head region at a rate proportional to remaining oxygen. Full oxygen: 1 bubble every 60 frames. Half oxygen: 1 bubble every 30 frames. Critical oxygen: 1 bubble every 10 frames. Bubbles are 1px, palette-appropriate (light blue-white). |

**Feel context modifiers for SWIMMING_SUBMERGED:**

| Context | Modification |
|---------|-------------|
| CALM | As specified. Smooth undulation. |
| TENSE | Undulation amplitude increases (body curves more dramatically). Stroke rate increases. Bubble rate doubles (stress breathing). |
| PANIC | Maximum stroke rate. Body writhes rather than undulates -- less efficient-looking movement. Bubble rate triples. |

---

**STATE: LANDING_RECOVERY**

| Aspect | Specification |
|--------|---------------|
| Medium landing | 4 frames (non-looping). F1: Impact -- body compresses 2px vertically, limbs absorb shock (splay outward). F2: Settle -- body begins to rise, legs re-center. F3: Recovery -- body at 80% standing height. F4: Stand -- full standing height restored, blends to IDLE. |
| Hard landing | 6 frames (non-looping). F1: Impact -- body compresses 3px, limbs buckle outward. F2-F3: Stumble -- body rocks forward, weight off-center, creature nearly falls. Head dips below body line. F4-F5: Recovery -- creature catches itself, weight re-centers. F6: Stand -- returns to IDLE height with 1-frame delay (the creature exhales). |
| Stagger landing | 8 frames (non-looping). F1: Impact -- maximum compression, body hits ground plane. Dust VFX triggers (see VAR-4). F2-F3: Collapse -- body slumps to one side. Asymmetric posture (damage frame per art bible: body folds around impact). F4-F5: Struggle -- creature pushes up from prone, 1px at a time. F6-F7: Rise -- unsteady return to standing. F8: Upright with visible residual unsteadiness (1px lateral offset from center). |

**Feel context modifiers for LANDING_RECOVERY:**

| Context | Modification |
|---------|-------------|
| CALM | As specified for each tier. |
| TENSE | Medium landing compresses to 3 frames (faster absorption). Hard landing recovery frames are faster. Creature returns to alert posture (ridge up) immediately on standing. |
| PANIC | Hard landing recovery is 4 frames (7 game-frame duration per CR-11 feel layer). Stagger recovery is 6 frames (14 game-frame duration). The creature forces itself up faster -- animation is jerkier, less graceful, but gets to locomotion sooner. |

---

**STATE: STUNNED**

| Aspect | Specification |
|--------|---------------|
| Base frame count | 4 frames (looping for stun duration) |
| Cycle duration | 0.4s at 10fps |
| Key poses | F1: Recoil -- body displaced in the direction of the external force. Limbs trail behind. Ridge erect (shock reflex). F2: Tumble -- body rotates 1px from upright (the creature is disoriented). F3: Recovery attempt -- body tries to right itself, partial success. F4: Relapse -- body settles back into disoriented posture. |
| Silhouette requirement | The creature's normal posture is broken. The silhouette reads as "not in control." Limbs are not in functional positions. The body axis is wrong. |
| Duration-driven | Loop repeats for the stun duration (set by the force source). On the final loop, F3 succeeds and F4 transitions to IDLE. |

---

**STATE: DEAD**

| Aspect | Specification |
|--------|---------------|
| Frame count | 15 frames (non-looping), playing over 30 game frames (500ms per CR state table) |
| Key poses | F1-F3: Collapse -- body loses all structural integrity. Limbs go limp. Ridge goes dark (final ridge pixel extinguishes last, per art bible section 7.1). F4-F8: Scatter -- body pixels begin to separate outward from center mass. 1-2 pixels detach per frame, drifting in random directions at 10-20px/s. F9-F15: Dissolution -- remaining pixels scatter as spore-like particles. Each pixel fades over 8 frames from body color to A1 (Void, the darkest palette neutral). |
| Art bible mandate | Death is not a screen. It is the last pixel of the dorsal ridge going dark. Camera holds position (Uninvited Frame). World continues. No fade to black. The dissolution takes 500ms, then the world runs for 2 full seconds before `request_respawn` fires. |
| No feel context variants | Death animation is the same regardless of context. The creature dies the same way whether calm or panicked. |

---

#### VAR-1.3 Turnaround Animation

When the creature reverses horizontal direction at >50% max speed (CR-2), the controller enters a 4-frame skid (67ms at CALM, down to 2 frames at PANIC).

| Aspect | Specification |
|--------|---------------|
| Frame count | 2 frames (non-looping, synced to skid duration) |
| Key poses | F1: Lean -- body mass continues in the original direction while limbs brake. The body leads the turn (per CR-2: "the creature's body leads the turn"). Center of mass offset 1-2px from limb contact points. F2: Pivot -- body snaps to new facing direction. Limbs reposition. 1px dust puff from contact point on dry surfaces. |
| Feel context scaling | At CALM (4 game frames), both animation frames hold for 2 game frames each. At PANIC (2 game frames), each animation frame holds for 1 game frame. The animation compresses but both poses are always present. |

---

#### VAR-1.4 Transition Animations

The following state transitions have dedicated transition frames (not just pop-cuts between states):

| Transition | Frames | Description |
|------------|--------|-------------|
| Stand to Crawl | 2 frames, 67ms | Body compresses. Limbs splay outward. Collider reshape syncs to F2 completion. |
| Crawl to Stand | 2 frames, 67ms | Reverse of above. Body rises. Only plays when ShapeCast confirms clearance. |
| Crawl to Squeeze | 3 frames, 100ms | Further compression. Body distorts to minimum dimensions. Plays during the 6-frame collider compress (CR-8b). |
| Ground to Wall Slide | 1 frame | Body rotates from horizontal to wall-contact orientation. Head reorients. |
| Wall Slide to Climb | 1 frame | Limbs shift from slide grip to climb splay. Body adjusts pressure against surface. |
| Climb to Ledge Mantle | 4 frames, 67ms | Leading limbs reach over ledge edge. Body hauls up. Rear limbs push. Body settles onto ledge surface. Per CR-5 auto-mantle. |
| Ground to Water Enter | 2 frames | Body transitions from ground stance to float posture. Splash VFX triggers on F1 (see VAR-4). |
| Water to Ground Exit | 3 frames, 100ms | Per CR-10 auto-climb-out. Limbs reach over edge, body hauls out, water drips briefly (1-2px particles). |

---

#### VAR-1.5 Animation Frame Budget Summary

Total unique animation frames for the base 16x16 sprite (both facing directions):

| Category | Frames per direction | x2 for facing | Subtotal |
|----------|---------------------|---------------|----------|
| IDLE (CALM) | 8 | 16 | 16 |
| IDLE (TENSE) | 4 | 8 | 8 |
| IDLE (SHELTERED) | 16 | 32 | 32 |
| WALKING | 3 | 6 | 6 |
| RUNNING (CALM/TENSE) | 4 | 8 | 8 |
| RUNNING (PANIC) | 6 | 12 | 12 |
| JUMPING | 3 | 6 | 6 |
| FALLING | 2 | 4 | 4 |
| WALL_SLIDING | 3 | 6 | 6 |
| WALL_CLINGING | 4 | 8 | 8 |
| WALL_JUMPING | 3 | 6 | 6 |
| CLIMBING | 4 | 8 | 8 |
| CRAWLING | 3 | 6 | 6 |
| SQUEEZING | 2 | 4 | 4 |
| SWIMMING_SURFACE | 4 | 8 | 8 |
| SWIMMING_SUBMERGED | 4 | 8 | 8 |
| LANDING_RECOVERY (medium) | 4 | 8 | 8 |
| LANDING_RECOVERY (hard) | 6 | 12 | 12 |
| LANDING_RECOVERY (stagger) | 8 | 16 | 16 |
| STUNNED | 4 | 8 | 8 |
| DEAD | 15 | 30 | 30 |
| Turnaround | 2 | 4 | 4 |
| Transitions (sum) | ~16 | 32 | 32 |
| **TOTAL (16x16 base)** | | | **~244 frames** |

At 16x16 pixels per frame with 1px gutter, each sprite sheet row fits ~120 frames in a 2048px sheet. The base creature requires approximately 3 sprite sheets (2048x16 each), well within the art bible's 2048px max sheet width and VRAM budget.

**Mid-evolution (24x24):** Frame counts increase by ~40% (add 1-2 frames per cycle). Estimated ~340 frames. 3-4 sheets at 2048x24.

**Late evolution (32x32):** Frame counts increase by ~80% over base (add 2-4 frames per cycle). Estimated ~440 frames. 4-5 sheets at 2048x32.

**Solo dev priority**: Implement CALM variants for all 16 states first. Add TENSE and PANIC variants for IDLE, RUNNING, FALLING, and WALL_SLIDING second (these are the most feel-critical). Add remaining context variants third. SHELTERED idle is high-value-low-effort (single long cycle, used in every rest).

---

### VAR-2. Sprite Degradation (Health Visualization)

Health is communicated entirely through visible damage to the creature sprite. The Player Controller exposes `health_current` and `health_max` to the Diegetic UI system (INT-5), which drives the degradation shader/procedural system. The degradation is per-pixel procedural, not pre-drawn sprite swaps (art bible section 7.1).

---

#### VAR-2.1 Degradation Tier Specifications

**Tier 1: INTACT (100-76% HP)**

| Aspect | Specification |
|--------|---------------|
| Color | All pixels at palette-correct saturation. No modification. |
| Silhouette | Full, unbroken outline. All limb and body pixels present. |
| Dorsal ridge | Shimmer pulse every 90 frames (1.5s). Shimmer is a 1-frame brightness increase of +1 palette step on all ridge pixels, then return. |
| Animation | All cycles play at full frame count with clean transitions. |
| Functional read | "Healthy animal." No visual stress. |

**Tier 2: STRESSED (75-51% HP)**

| Aspect | Specification |
|--------|---------------|
| Color | Outermost pixel layer (the 1px contour ring of the body, excluding dorsal ridge) loses 1 saturation step. Interior pixels unchanged. The edge of the creature looks slightly washed. |
| Silhouette | Intact. No pixel loss. The shape is complete but the edge is dimmer. |
| Dorsal ridge | Shimmer slows to every 140 frames (2.33s). The creature's vitality indicator is sluggish. |
| Animation modification | One limb animation drops 1 frame from its cycle. For locomotion states: the trailing limb's contact frame (the last ground-touch frame in the walk/run cycle) is removed. The creature develops a subtle limp -- one footfall is lighter than the other. This affects WALKING and RUNNING only. |
| Functional read | "Something is wrong. Watch more carefully to see what." The player must actively observe to detect the change. |

**Tier 3: WOUNDED (50-26% HP)**

| Aspect | Specification |
|--------|---------------|
| Color | Two saturation steps lost across the full body (not just the contour). The creature is visibly desaturated -- still recognizable within its palette but trending toward neutral. Dorsal ridge retains full saturation (it is the last element to degrade per art bible). |
| Silhouette | One pixel of the silhouette boundary "chips" -- a single pixel gap appears on one limb or the tail edge. This gap is permanent for the tier (does not animate open/closed). The pixel that was there has gone dark. The creature's outline is broken. |
| Dorsal ridge | Static. No shimmer at all. The ridge is visible but inert -- no pulse, no brightness cycle. |
| Animation modification | Movement trail shortens by 50% (if movement trail is implemented as diegetic bioluminescent residue). Idle animation gains a flinch frame: every 4th idle cycle, the body compresses 1px involuntarily (pain response). Walk and run animations lose the limp frame from Stressed AND an additional frame: walk becomes 2 frames (less fluid), run becomes 3 frames (losing the gather frame, making the run look labored). |
| Functional read | "This creature is hurt. It is visibly damaged. Moving is costing it." A player at Wounded tier should feel the urge to find shelter. |

**Tier 4: CRITICAL (25-1% HP)**

| Aspect | Specification |
|--------|---------------|
| Color | Three saturation steps lost across the full body. The creature approaches grayscale -- recognizable by shape and ridge only, not by color. Body pixels flicker: every 6 game frames, 2-3 random body pixels flash to -20% value for 2 frames, then return. The creature is visually unstable. |
| Silhouette | 2-3 pixel gaps in the outline. Multiple limb and edge pixels are dark. The creature's silhouette is visibly degraded -- it reads as damaged, incomplete. |
| Dorsal ridge | Pixels flicker erratically. Each ridge pixel independently cycles on/off on a randomized 8-16 frame timer. The ridge is still identifiable by its position but its pixels are unreliable. This flicker syncs with the shelter-direction indicator (art bible section 7.5: the compass becomes unreliable at Critical). |
| Animation modification | All locomotion animations lose 2 frames. Walk is 1 frame (a lurching step). Run is 2 frames (stumbling sprint). Jump loses the anticipation frame (the creature can barely gather itself). A single trailing pixel detaches from the body during movement -- 1px particle separates from a random body-edge pixel on each locomotion cycle, drifts 4-8px in the wake direction, and fades over 8 frames from body color to A1 (Void). The creature is literally losing itself. |
| Functional read | "This creature is dying. Every movement costs it mass. It must reach shelter." The visual distress should be impossible to ignore. |

---

#### VAR-2.2 Degradation Pixel Priority Map

Damage affects pixels in a weighted priority order (extremities first, core last):

| Priority (first to degrade) | Pixel Region | Weight |
|------------------------------|-------------|--------|
| 1 (first) | Tail/trailing element tips | 1.0 |
| 2 | Outer limb tips | 0.9 |
| 3 | Ear/sensory structure tips | 0.85 |
| 4 | Body contour edge pixels | 0.7 |
| 5 | Limb mid-sections | 0.5 |
| 6 | Body interior pixels | 0.3 |
| 7 | Head pixels | 0.15 |
| 8 (last) | Dorsal ridge pixels | 0.05 |

When damage is received, the Diegetic UI selects the highest-priority pixel region that has not yet fully degraded and applies the next saturation step reduction. This creates a naturalistic injury pattern: the extremities show damage first, the core holds longest, and the ridge is the creature's last living signature.

---

#### VAR-2.3 Degradation Interaction with Animation

Degradation is not just cosmetic overlay -- it modifies animation behavior:

| Health Tier | Walk Frames | Run Frames | Jump Anticipation | Idle Cycle | Ridge State |
|-------------|-------------|------------|-------------------|------------|-------------|
| Intact | 3 | 4 | 2 game frames | 8 frames, full | Shimmer @90f |
| Stressed | 3 (with limp) | 4 (with limp) | 2 game frames | 8 frames, full | Shimmer @140f |
| Wounded | 2 | 3 | 2 game frames | 8 frames + flinch every 4th cycle | Static |
| Critical | 1 | 2 | 0 (no anticipation) | 4 frames, truncated | Erratic flicker |

**Degradation recovery**: In shelter, pixels recover at 1 saturation step per 3 seconds of rest. Animation frames are restored as the creature crosses tier thresholds upward. The recovery is visible: the creature gradually regains color, its outline repairs, its movement smooths out. This gives the shelter rest tangible visual weight.

---

### VAR-3. Evolution Visual Variants

The Player Controller exposes `evolution_hooks` (CR-9), which the Player Visual system reads to select sprite sets and apply visual modifications. Evolution permanently modifies the creature's sprite. Each mutation adds, reshapes, or recolors pixels on the base form.

---

#### VAR-3.1 Mutation Visual Regions

The creature's sprite is divided into modifiable regions. Each evolution category targets specific regions:

| Region | Pixel Area (16x16) | Pixel Area (32x32) | Modifiable By |
|--------|--------------------|--------------------|---------------|
| **Dorsal ridge** | 3-6 px along spine | 8-16 px along spine | All evolutions (grows in complexity per art bible 5.1.3). Never removed. |
| **Head/sensory apparatus** | 4-6 px upper-forward | 12-20 px upper-forward | Sensory evolutions (echolocation, enhanced vision, heat pits) |
| **Forelimbs** | 2-4 px lower-forward | 6-12 px lower-forward | Grip evolutions (gecko pads, claw development), attack evolutions |
| **Hindlimbs** | 2-4 px lower-rear | 6-12 px lower-rear | Speed evolutions (hardened foot pads), jump evolutions |
| **Flanks/torso** | 6-10 px mid-body | 16-30 px mid-body | Thermal evolutions (heat-dissipation fins), armor evolutions, gill slits |
| **Tail/trailing element** | 2-4 px rear | 6-10 px rear | Aquatic evolutions (tail membrane), balance evolutions |
| **Ventral surface** | 3-5 px underside | 8-14 px underside | Burrowing evolutions, toxin resistance (color shift) |

---

#### VAR-3.2 Evolution Category Visual Effects

| Evolution Category | Visual Modification | Biome Color Source | Silhouette Change |
|--------------------|--------------------|--------------------|-------------------|
| **Thermal adaptation** | Heat-dissipation fins: 2-3px protrusions on flanks. Pixel color shifts to amber-heat tones from the server farm biome palette. | Warm/amber (H5-H8 range from decay ramp) | Wider silhouette. Textured flank edge. Vertical height may increase. |
| **Aquatic adaptation** | Tail membrane: posterior silhouette widens 2-4px. Gill slits: 2px horizontal lines on thorax. Limbs may develop webbing (1px between digit pixels at 24x24+). | Cool blue-green (B1-B4 from biological ramp) | Elongated horizontally. Smoother contour (aquadynamic). |
| **Fungal/spore adaptation** | Spore membrane: 1-2px translucent overlay on body surface. Texture fills negative space between body segments. Bioluminescent pixel accents. | Teal-white (T1-T3 from transitional ramp) | Negative space fills in. Body becomes more textured, less smooth. |
| **Grip/traversal** | Setae on limbs: 1px protrusions on forelimb and hindlimb edges. At 24x24+, individual grip structures visible. | Surface-matched (matches wall material colors) | Limbs become more complex, more angular terminal points (predator-adjacent, per art bible 3.1.3). |
| **Sensory enhancement** | Head region expands: antennae, whisker structures, or heat pit depressions. 1-2px additions to head mass. | Biome of trigger (varies) | Head becomes more prominent. Sensory structures may break the forward silhouette. |
| **Armor/resistance** | Body contour thickens: 1px of armor plating replaces soft body edge. Color shifts toward harder, less saturated values. | Muted neutrals (N1-N3 from neutral ramp) | Denser silhouette. Less negative space. Body reads as sturdier. |

---

#### VAR-3.3 Evolution Layering with Health Degradation

Evolution visuals and health degradation interact through the per-pixel procedural system:

| Principle | Rule |
|-----------|------|
| **Evolved pixels degrade like body pixels** | Mutation-added pixels follow the same degradation priority map (extremities first, core last). Heat fins at the body edge degrade before body core pixels. |
| **Biome colors desaturate on the same curve** | Evolution-sourced colors (amber from thermal, teal from fungal) lose saturation steps at the same rate as base body colors. A critically damaged thermally-adapted creature's fins are near-gray, not amber. |
| **Dorsal ridge evolution is protected** | The ridge's evolved complexity (serrated, branching, etc.) follows the same "last to degrade" rule as the base ridge. Even at Critical, the ridge's evolved shape persists -- it flickers erratically but retains its silhouette profile. |
| **Structural additions do not chip** | The silhouette-gap "chipping" at Wounded/Critical tiers preferentially targets base-form pixels, not evolution-added structures. Rationale: the adaptations are newer tissue, the base form is older and more degraded. This means a heavily evolved creature at Critical health looks paradoxical -- the original body is crumbling but the adaptations are holding. This is intentional and thematically correct: "You Become What You Survive." |
| **Trailing pixel shedding uses body color, not evolution color** | At Critical tier, the 1px trailing particles that detach during movement use the creature's base body color, not the evolution-region color. The creature is losing its original self, not its adaptations. |

---

#### VAR-3.4 Sprite Sheet Complexity Estimation

Maximum sprite sheet requirements across all evolution states:

| Evolution State | Sprite Size | Frames (est.) | Sheet Count (2048px wide) | VRAM per sheet |
|----------------|-------------|---------------|---------------------------|----------------|
| Base (no evolutions) | 16x16 | ~244 | 3 sheets (2048x16) | ~96 KB |
| Early (1-2 evolutions) | 20x20 | ~290 | 3 sheets (2048x20) | ~150 KB |
| Mid (3-4 evolutions) | 24x24 | ~340 | 4 sheets (2048x24) | ~260 KB |
| Late (5-6 evolutions) | 32x32 | ~440 | 5 sheets (2048x32) | ~450 KB |

**Total VRAM for all player sprite sheets**: ~956 KB (under 1 MB). Well within the 128 MB VRAM ceiling.

**Solo dev strategy for evolution variants**: Do not pre-draw every evolution combination. Instead:
1. Create the base sprite sheets for each size tier (16x16, 24x24, 32x32).
2. Create modular overlay sheets for each evolution category (thermal fins, gill slits, sensory structures, etc.) at each size tier.
3. Composite at runtime: the Player Visual system layers the base sprite + active evolution overlays. This reduces the combinatorial explosion from hundreds of full sheets to ~6 overlay sets per size tier.
4. Evolution overlays are 1-3 pixel modifications per region -- small enough to be additional draw calls within the 120 draw-call budget (each overlay composites via a shader or secondary sprite node, not a separate draw call if done via shader).

---

### VAR-4. Surface Interaction VFX

All surface VFX are driven by the Player Controller's state and surface data. The Player Visual system or a dedicated VFX node reads `current_state`, `velocity`, `is_on_floor`, `wall_direction`, and surface type from the controller. All particles are 1x1 or 2x2 palette-colored rects (art bible section 8.8). Maximum 1 player-interaction emitter active at a time (the player's VFX emitter is highest priority in the 6-emitter budget).

---

#### VAR-4.1 Footstep Particles

Triggered on each locomotion frame that includes a ground-contact pose (the "contact" frames in walk/run cycles).

| Surface Type | Particle Color | Particle Count | Particle Size | Behavior |
|-------------|---------------|----------------|---------------|----------|
| Dry Stone | Warm gray (H6 Dust) | 2-3 per step | 1x1 | Rise 2-4px, drift horizontally 1-2px in movement direction, fade over 10 frames. |
| Wet Stone | Blue-gray (N2) | 1-2 per step | 1x1 | Tiny splash: rise 1-2px, fall back to surface over 6 frames. No horizontal drift. |
| Metal (dry) | None | 0 | -- | No visible dust. Metal is clean. Audio only (see VAR-5). |
| Metal (wet) | Blue-gray (N2) | 1 per step | 1x1 | Single drip splash. Rise 1px, fall immediately. |
| Moss | Green (B3 Moss) | 1-2 per step | 1x1 | Tiny fiber particles. Rise 1-3px, drift slowly, fade over 16 frames (hang in air longer). |
| Dense Vine | Green-brown (T2 Rust Moss) | 1 per step | 1x1 | Fibrous debris. Falls downward from contact point (vine is above), fades over 8 frames. |
| Fungal Mat | Teal-white (B7 Spore) | 2-3 per step | 1x1 | Spore puff. Rise 3-5px in a slow upward drift, persist for 20 frames. Visually distinct from other surfaces. |
| Root Structure | Brown (H7 Soil) | 1-2 per step | 1x1 | Dirt particles. Behave like Dry Stone but with brown coloring. |
| Ice | None | 0 | -- | No particles. The surface is frictionless. Scratch marks are not visible at pixel scale. |
| Crumbling | Warm gray (H6 Dust) | 4-6 per step | 1x1 to 2x2 | Heavy debris. Particles fall downward from the surface, accelerating at 200px/s^2. Larger count communicates surface instability. Intensifies as the 0.5s break timer progresses. |

**Cadence**: Walk generates particles on F1 (contact frame) only -- 1 emission per cycle. Run generates on F1 and F3 (both contact frames) -- 2 emissions per cycle. Crawl generates a continuous low-rate emission (1 particle per 6 game frames) as the body drags. Squeeze generates crumbling-type debris continuously.

---

#### VAR-4.2 Landing Impact Particles

Triggered when the creature transitions from FALLING to a ground state. Particle count and behavior scale with landing severity.

| Landing Tier | Particle Count | Particle Size | Rise Height | Spread Radius | Fade Duration |
|-------------|---------------|---------------|-------------|---------------|---------------|
| Soft (<200 px/s) | 0-1 | 1x1 | 1px | 2px | 6 frames |
| Medium (200-300 px/s) | 3-4 | 1x1 | 3-4px | 4px | 10 frames |
| Hard (300-350 px/s) | 6-8 | 1x1 to 2x2 | 5-8px | 8px | 14 frames |
| Stagger (350+ px/s) | 10-12 | 2x2 | 8-12px | 12px | 18 frames |

Particle color is determined by the surface type landed on (same colors as footstep table above). For stagger landings, the outermost 2-3 particles use A1 (Void) regardless of surface -- these represent the creature's own body debris.

Impact particles spray outward from the contact point in a 180-degree arc (upward and to both sides). Horizontal spread is proportional to `abs(velocity.x)` at impact -- a creature that falls straight down produces a symmetrical spray; one that was moving laterally produces a directional spray.

---

#### VAR-4.3 Wall Slide Particles

Triggered continuously during WALL_SLIDING state. Particles emit from the contact point between creature and wall surface.

| Aspect | Specification |
|--------|---------------|
| Particle rate | 1 particle per 4 game frames (15/second) |
| Particle color | Matches wall surface material (stone gray for stone, green for vine, brown for root, teal for fungal) |
| Particle size | 1x1 |
| Behavior | Particles fall downward from the contact point at the wall slide speed. They hug the wall surface (no horizontal drift away from wall). Fade over 8 frames. |
| Grip degradation visual | After 2.0s of wall sliding, particle rate doubles (2 per 4 game frames) and particles gain 1-2px horizontal drift away from wall. This communicates the grip degradation visually -- more debris means the creature's grip is failing. |

---

#### VAR-4.4 Water Interaction VFX

**Water surface entry (from ground):**
- Splash ring: 4-6 particles, 1x1, arranged in a semicircle above the entry point. Rise 3-6px, arc outward 2-4px, fall back to water surface and disappear. Color: water palette of current biome (cool blue-green range).
- Surface ripple: A 1px wave displacement on the water surface tile, radiating 8-16px outward from the entry point over 12 frames, then settling.

**Water surface entry (from fall):**
- Scales with fall velocity. Soft fall: same as ground entry. Medium fall: double the particle count. Hard/stagger fall: triple count, particles rise 8-12px, surface ripple extends 16-24px.

**Surface swimming:**
- Wake particles: 1 particle per stroke (per swim animation F2). 1x1, spawns at rear of creature, drifts 2-4px opposite to movement direction, fades over 10 frames.
- Surface disturbance: 1px wave displacement at creature position, persistent during movement, settles within 4 frames of stopping.

**Submerged swimming:**
- Bubble trail: 1-2px bubbles rise from creature's head region. Rate proportional to oxygen (see VAR-1.2 SWIMMING_SUBMERGED). Bubbles rise at 30-40px/s toward the surface, sway laterally 1-2px on a sine wave. Bubbles pop (disappear in 1 frame) on reaching the water surface.
- Movement disturbance: No wake particles underwater. Movement is silent and clean (the creature is adapted to this medium if it can submerge).

**Water exit:**
- Drip particles: 2-3 particles of water color cling to the creature sprite for 20-30 frames after exiting water, then fall as 1px drops at 100px/s. This is the wet-shader interacting with the creature's contour (per art bible section 2.7: surface pixels darken 10-15% and gain +5% saturation when wet). The wet effect fades over 2 seconds after exit.

---

#### VAR-4.5 Squeeze Passage VFX

| Aspect | Specification |
|--------|---------------|
| Particle type | Crumbling debris (same as Crumbling surface footstep particles) |
| Emission rate | 1-2 particles per game frame (continuous, high density) |
| Emission point | Both above and below the creature (the passage is tight on all sides) |
| Particle behavior | Particles fall from above (ceiling debris) and rise from below (floor displacement). Spread is tight -- 2-3px maximum from the creature's center. Color: matches passage material. |
| Dust cloud | A persistent 2x2 opacity-reduced (50% alpha) dust cluster follows the creature through squeeze passages, trailing 4-8px behind. Fades within 6 frames of exiting the squeeze. |

---

### VAR-5. Audio Cues (Controller-Driven)

The Player Controller does not play audio directly. The Player Visual System emits animation-frame signals (e.g., `contact_frame_reached`, `impact_frame_reached`) that the Audio system listens to. The Audio system reads `current_state`, `threat_context`, and surface type to select the correct sound. All audio specifications below describe what the Audio system must play in response to controller state.

**Master audio principle**: All player-creature sounds are diegetic -- they are sounds this creature physically makes. No musical stings, no UI sound effects, no abstract feedback tones. The creature's audio footprint is small: it is a small animal and it sounds like one.

---

#### VAR-5.1 Footstep Audio

Footstep sounds trigger on contact frames of walk and run animations (same frames that trigger footstep particles).

**Cadence by state:**

| State | Steps per second (at base speed) | Character |
|-------|----------------------------------|-----------|
| WALKING | ~3.3/s (1 per walk cycle at 0.3s) | Light, tentative taps |
| RUNNING | ~5.0/s (2 per run cycle at 0.4s) | Rapid, committed impacts |
| CRAWLING | Continuous scrape, no discrete steps | Dragging body texture |
| CLIMBING | ~2.5/s (1 per climb cycle at 0.4s) | Grip-and-release, surface-dependent |

**Surface type variation:**

| Surface | Sound Character | Volume (relative) |
|---------|----------------|-------------------|
| Dry Stone | Sharp, clicky. High-frequency tap. | 1.0 (baseline) |
| Wet Stone | Muffled click with micro-splash. Slight reverb. | 0.9 |
| Metal (dry) | Metallic ping. Bright, resonant. | 1.1 (metal carries sound -- this creature is louder on metal, which has gameplay implications for stealth) |
| Metal (wet) | Dulled metallic slap. Squelch undertone. | 0.8 |
| Moss | Near-silent. Soft compression. Barely audible below running speed. | 0.4 |
| Dense Vine | Organic creak. Fibrous snap. | 0.7 |
| Wet Vine | Slippery organic squeak. | 0.6 |
| Fungal Mat | Soft, spongy thud. Low-frequency. | 0.5 |
| Root Structure | Woody click. Hollow tap. | 0.8 |
| Ice | Glass-like scrape. High, thin. | 0.9 |
| Crumbling | Grinding, gritty. Debris crackle. Intensifies as surface approaches breakpoint. | 1.2 |
| Rope/Cable | Fibrous creak and tension sound. | 0.7 |

**Feel context modification:**

| Context | Audio Modification |
|---------|-------------------|
| CALM | Baseline volume and character. |
| TENSE | Volume reduced 20% across all surfaces. Footsteps become more careful -- the creature is trying to be quiet. High-frequency components attenuated. |
| PANIC | Volume increased 15%. Footsteps are heavier, more desperate. Low-frequency impact component added. Steps may be slightly irregular in timing (1-2 frame jitter, mirroring the desperate run animation). |
| PRECISE | Volume reduced 30%. Each step is crisp, deliberate, isolated. Longer silence between steps. |
| SHELTERED | Volume reduced 40%. Softest possible footfall. Warm, padded quality. |

---

#### VAR-5.2 Breathing Audio

The creature's breathing is an ambient audio loop that shifts with feel context. It is always present but often at the threshold of audibility. Breathing is the creature's most personal sound -- the one audio element that is purely "you."

| Context | Breathing Character | Cycle Rate | Volume |
|---------|--------------------| -----------|--------|
| CALM | Slow, even, barely perceptible. Nasal inhale/exhale with slight organic rasp. | ~4s per breath cycle | 0.15 (near-subliminal) |
| TENSE | Faster, shallower. Held-breath quality -- the exhale is shorter than the inhale. Audible enough to create unease. | ~2.5s per cycle | 0.3 |
| PANIC | Rapid, ragged. Open-mouth gasping. Audible over footsteps. This is the sound of a small animal sprinting for its life. | ~1.2s per cycle | 0.6 |
| PRECISE | Controlled, measured. Deliberate inhale-hold-exhale pattern (the creature is concentrating). | ~3s per cycle | 0.2 |
| SHELTERED | Deep, slow, relaxed. The longest exhale in the game. Body settling sounds (micro-shifts, soft compression). | ~6s per cycle | 0.2 |

**Health interaction:** At Wounded tier, breathing gains a wheeze -- a mid-frequency roughness on the exhale. At Critical tier, breathing becomes arrhythmic (random 20-40% variation in cycle timing) and gains an audible catch/stutter every 3rd cycle.

**Context transition:** Breathing transitions smoothly between contexts. The current breath cycle completes before the new rhythm begins (no mid-breath switching). Exception: transition TO PANIC interrupts immediately -- the gasp cuts in regardless of current breath phase.

---

#### VAR-5.3 Landing Impact Audio

Triggered on state transition from FALLING to ground state. Scaled by landing severity.

| Landing Tier | Sound Character | Volume | Additional |
|-------------|----------------|--------|-----------|
| Soft (<200 px/s) | Light tap. Barely audible. Single surface-appropriate impact. | 0.3 | None. |
| Medium (200-300 px/s) | Solid thud. Body weight audible. Short exhalation grunt from creature. | 0.6 | 50ms of settling debris sound (surface-dependent). |
| Hard (300-350 px/s) | Heavy impact. Full body slam. Audible stumble sounds (scraping, sliding). Creature grunt is louder and pained. | 0.8 | 150ms of debris. Creature pain vocalization (short, animal distress -- not a human sound). |
| Stagger (350 px/s, 1 HP damage) | Crash. Maximum impact. Body hits surface with full mass. Pain vocalization is sharp and involuntary. | 1.0 | 300ms of debris settling. A beat of silence after the crash (the creature is stunned). Then breathing resumes at elevated rate. |

Surface material modifies the impact timbre (stone is hard and sharp, moss is dull and soft, metal rings, fungal mat absorbs), using the same tonal character as footsteps but at proportionally greater volume and with more low-frequency content.

---

#### VAR-5.4 Wall Interaction Audio

| Event | Sound | Volume | Notes |
|-------|-------|--------|-------|
| Wall slide initiation | Contact scrape -- the creature's body pressing against surface. Brief (100ms). | 0.5 | Surface-dependent timbre (stone grinds, vine crackles, root creaks). |
| Wall slide continuous | Continuous low scrape, pitch proportional to slide speed. At base 80px/s, low-frequency rumble. At grip-degraded higher speeds, pitch rises. | 0.3 (continuous) | Cuts immediately on state change. |
| Wall cling | Near-silence. Occasional grip-settle sounds (every 2-3 seconds, a tiny creak or shift). Breathing is the dominant audio in cling state. | 0.1 (grip sounds) | Emphasizes the creature's vulnerability -- you hear yourself breathing on the wall. |
| Wall jump | Sharp push-off sound. Material-dependent (stone: scrape; vine: snap; root: creak). 50ms duration. | 0.6 | Volume increases slightly with each consecutive wall jump (the creature pushes harder as fatigue increases). |
| Grip degradation | After 2.0s wall slide, the scraping sound gains intermittent high-frequency squeaks (slipping). Rate increases as grip fails. | +0.1 above base | Audio warning that grip is failing -- pairs with the visual particle increase. |

---

#### VAR-5.5 Water Audio

| Event | Sound | Volume | Notes |
|-------|-------|--------|-------|
| Water surface entry (soft) | Small splash. Brief water displacement. | 0.4 | Gentle plop. |
| Water surface entry (fall) | Scaled splash. Medium fall: moderate splash with brief submersion muffling. Hard/stagger fall: heavy splash with 200ms of muffled audio (the creature goes briefly under). | 0.5-0.9 | Scales with impact velocity. |
| Surface swimming | Rhythmic paddling. Small lapping sounds synced to stroke animation frames. | 0.3 | Continuous ambient water-surface texture underneath. |
| Submerge | Muffled transition. All audio passes through a low-pass filter (simulating underwater hearing). Creature sounds become dull, distant. | -- | Global audio filter, not a specific sound. |
| Submerged swimming | Muffled movement. Water displacement. Bubble sounds synced to bubble particle emissions. | 0.3 (filtered) | The quietest locomotion state in the game. The creature is in an alien medium. |
| Surface breach | Water breaking. Brief rush of un-filtered audio (the muffling lifts). Small gasp as the creature breathes. | 0.5 | The return of clear audio is itself a relief cue. |
| Water exit | Water dripping from body. 3-5 drip sounds over 2 seconds (synced to the drip particle VFX). Surface footsteps gain a wet quality for 2 seconds after exit. | 0.3 | Wet footstep modification fades over the same 2-second window as the visual wet-shader. |

---

#### VAR-5.6 Squeeze Passage Audio

| Aspect | Specification |
|--------|---------------|
| Entry sound | Compression crunch. The creature's body pressing into a tight space. Material-dependent (stone: grinding, vine: organic squelch, fungal: soft compression). 200ms duration. |
| Continuous sound | Low, strained scraping. The creature dragging itself through a gap too small for comfort. Volume proportional to squeeze speed (louder when moving, near-silent when stopped). Overlaid with creature strain vocalization -- a continuous, quiet effort sound (not a grunt, more like sustained tension in the throat). |
| Breathing override | In squeeze state, breathing becomes the loudest creature sound. Rapid, shallow, claustrophobic. Cycle rate: 0.8s (faster than PANIC breathing, because PANIC is open-air terror; squeeze is enclosed-space terror). |
| Exit sound | Release -- a brief exhalation and the sound of material settling as the creature expands back to normal dimensions. 100ms. |
| Volume | Entry: 0.5. Continuous: 0.2-0.4. Breathing: 0.5. Exit: 0.4. |

---

#### VAR-5.7 Jump Audio

| Event | Sound | Volume | Notes |
|-------|-------|--------|-------|
| Ground jump | Light push-off sound. Surface material determines timbre (stone: click, moss: silence, vine: snap). 50ms. | 0.4 | At PANIC context: louder (0.6), more explosive. At PRECISE: softer (0.3), more deliberate. |
| Variable jump (short hop) | Same as ground jump but with audible deceleration (a subtle "pulling back" quality as the creature cuts its upward momentum). | 0.3 | The early jump-release creates a shorter, more contained sound. |
| Wall jump | Combines push-off (from wall material) with a directional whoosh (50ms, low volume). | 0.5 | Volume increases 10% per consecutive wall jump (mirrors the increasing visual desperation). |
| Climb launch | Sharper than wall jump. The creature is releasing from a grip, not bouncing -- the sound has a "snap" quality (grip releasing). | 0.5 | Surface-dependent (vine snap, root creak, rope twang). |
| Jump from water surface | Splash + push-off combined. Water displacement sound dominates. Heavier than ground jump (water drag). | 0.5 | Less crisp than ground jump. The creature is fighting the water. |
| Apex silence | At the top of a jump arc (velocity.y crosses zero), a 2-frame audio dip occurs -- all creature sounds reduce by 30%. | -- | This micro-silence is subliminal but creates the feeling of weightlessness at the apex. |

---

#### VAR-5.8 Damage and Death Audio

| Event | Sound | Volume | Notes |
|-------|-------|--------|-------|
| Damage received | Sharp, involuntary pain vocalization. Animal distress -- NOT a human cry. Short (100-150ms). A yelp, chirp, or squeak appropriate to a small creature. | 0.7 | Sound does not vary by damage source -- the creature does not know what hit it, only that it hurts. |
| Health tier transition (downward) | No dedicated sound. The ongoing audio modifications (breathing changes, footstep changes) communicate the transition gradually. | -- | Abrupt damage sounds would create a "health bar tick" feeling. Gradual audio degradation mirrors gradual visual degradation. |
| Death | The damage vocalization, sustained and trailing off. 500ms. As the creature's pixels scatter, the vocalization fades and distorts (as if the creature's body is losing the ability to produce sound). The last audible element is a single breath that doesn't complete -- an inhale with no exhale. | 0.8 -> 0.0 | After the vocalization fades, world audio continues normally for the 2-second hold before respawn. The world does not mourn. Consistent with Pillar 1 and the Uninvited Frame. |
| Stun (external force) | Impact sound (determined by force source, not by the Player Controller). Creature produces a disorientation vocalization -- a warbled, unstable version of the pain sound, sustained for the stun duration. | 0.5 | The sustained quality distinguishes stun from damage. Damage is sharp and ends. Stun is ongoing. |

## UI Requirements

The Player Controller has **no direct UI elements**. All player-facing information is delivered through the Diegetic UI System (INT-5), which reads controller state and visualizes it through in-world means:

- **Health**: Sprite degradation tiers (4 levels, defined in Visual/Audio Requirements VAR-2). Owned by Diegetic UI, driven by `health_current` / `health_max` from controller.
- **Oxygen**: Bubble frequency from creature sprite when submerged. Owned by Diegetic UI, driven by `oxygen_current` from controller.
- **Direction**: Dorsal ridge compass (4x4 pixel element on creature sprite) pointing toward last shelter. Owned by Diegetic UI, derived from `global_position` relative to `last_shelter_position`.
- **Evolution status**: Reflection pool mechanic in shelters — creature's evolved form displayed in water reflection. Owned by Diegetic UI, reads `evolution_hooks` from controller.
- **Threat level**: Communicated entirely through feel layer effects (movement changes, animation changes, camera behavior, breathing audio). No UI element.

**No HUD elements exist for the Player Controller.** No health bar, no stamina bar, no oxygen meter, no minimap, no button prompts. This is a core commitment (Anti-pillar: NOT hand-holdy).

**Pause screen**: The game pauses as "world stillness" — the world freezes, audio drops to low-pass filtered ambience, and a minimal menu appears. The controller's state is frozen on pause entry and restored on unpause. Pause menu UI is owned by the Settings & Accessibility system, not the Player Controller.

> **UX Flag — Player Controller**: This system has no direct UI requirements, but the Diegetic UI system's health/oxygen/direction visualizations depend entirely on controller state. When authoring the Diegetic UI GDD, reference this document's INT-5 contract for the data interface.

## Acceptance Criteria

### 1. Movement Physics (Ground, Air, Jump Heights, Speeds)

**AC-1.1 (CR-1: Physics Foundation)**
GIVEN the game is running at the target 480x270 viewport resolution, WHEN a physics frame executes, THEN `_physics_process(delta)` runs at exactly 60fps (delta = 0.01667s +/- 0.0001s) and all velocity values are stored as a Vector2 in pixels per second.

**AC-1.2 (CR-1: Collision Shape)**
GIVEN the creature is in its default standing state with no evolution mutations, WHEN the creature's collider is inspected, THEN the capsule collider measures exactly 10px wide x 12px tall, smaller than the 16x16 sprite on all sides.

**AC-1.3 (CR-1: One-Way Platform Drop-Through)**
GIVEN the creature is standing on a tile flagged as one-way (vine, thin ledge, or fungal shelf), WHEN the player presses Down + Jump simultaneously, THEN collision with that platform is disabled for exactly 6 frames (100ms), the creature falls through, and collision re-enables after the 6th frame.

**AC-1.4 (CR-2: Walk/Run Speed)**
GIVEN the creature is standing on dry stone (friction 1.0) in CALM context with no evolution bonuses, WHEN the player holds full horizontal input (magnitude >= 0.4), THEN the creature accelerates to exactly 120 px/s and does not exceed 120 px/s.

**AC-1.5 (CR-2: Walk Speed at Low Input)**
GIVEN the creature is standing on dry stone in CALM context, WHEN the player applies horizontal input with analog magnitude of 0.39 (below 0.4 threshold), THEN the creature's target speed is 60 px/s (walk speed) and the creature does not enter run speed.

**AC-1.6 (CR-2: Ground Acceleration Timing)**
GIVEN the creature is at rest on dry stone in CALM context, WHEN the player holds full horizontal input, THEN the creature reaches 120 px/s (base run speed) from standstill in exactly 0.2 seconds (600 px/s^2 acceleration).

**AC-1.7 (CR-2: Ground Deceleration on Input Release)**
GIVEN the creature is running at 120 px/s on dry stone in CALM context, WHEN the player releases horizontal input, THEN the creature decelerates at 900 px/s^2 and reaches 0 px/s in 0.133 seconds.

**AC-1.8 (CR-2: Turnaround Deceleration)**
GIVEN the creature is running right at >50% max speed on dry stone in CALM context, WHEN the player presses left (input reversal), THEN the creature enters a 4-frame (67ms) skid animation and decelerates at 1200 px/s^2 before accelerating in the new direction.

**AC-1.9 (CR-3: Full Jump Height)**
GIVEN the creature is on dry stone in CALM context with no evolution bonuses, WHEN the player presses and holds the jump button until the apex, THEN the creature reaches a maximum height of 49px (+/- 1px) above the launch point, with a time-to-apex of 0.35 seconds.

**AC-1.10 (CR-3: Variable Jump Height -- Short Hop)**
GIVEN the creature is on dry stone in CALM context, WHEN the player taps the jump button and releases immediately (within 1 frame), THEN the remaining upward velocity is multiplied by 0.4 on the release frame, and the creature reaches a minimum height of approximately 20px (+/- 2px).

**AC-1.11 (CR-3: Descending Gravity)**
GIVEN the creature is airborne and has passed the jump apex (vertical velocity >= 0, meaning downward), WHEN gravity is applied on the next physics frame, THEN descending gravity is 1000 px/s^2, which is 200 px/s^2 greater than the ascending gravity of 800 px/s^2.

**AC-1.12 (CR-3: Terminal Velocity)**
GIVEN the creature is falling in CALM context with no evolution overrides, WHEN downward velocity reaches 350 px/s, THEN the velocity is capped and does not exceed 350 px/s regardless of continued falling duration.

**AC-1.13 (CR-3: Air Control)**
GIVEN the creature is airborne in CALM context, WHEN the player applies horizontal input, THEN horizontal acceleration is 420 px/s^2 (70% of 600) and horizontal deceleration is 270 px/s^2 (30% of 900).

**AC-1.14 (CR-11: Soft Landing)**
GIVEN the creature has been falling for less than 0.3 seconds with impact velocity below 200 px/s, WHEN the creature contacts a floor, THEN the creature transitions immediately to Idle/Walk/Run with 0 recovery frames and no penalty.

**AC-1.15 (CR-11: Medium Landing)**
GIVEN the creature has been falling for 0.3-0.6 seconds with impact velocity between 200-300 px/s, WHEN the creature contacts a floor, THEN a 4-frame (67ms) landing animation plays and the player can buffer inputs during those frames with no functional movement delay.

**AC-1.16 (CR-11: Hard Landing)**
GIVEN the creature has been falling for 0.6-1.0 seconds with impact velocity between 300-350 px/s, WHEN the creature contacts a floor, THEN horizontal velocity is zeroed, the creature enters a 10-frame (167ms) recovery during which no movement input is accepted, and a jump can be buffered starting on the landing frame with execution on frame 11.

**AC-1.17 (CR-11: Stagger Landing)**
GIVEN the creature has been falling for more than 1.0 seconds at terminal velocity (350 px/s), WHEN the creature contacts a floor, THEN horizontal velocity is zeroed, the creature enters an 18-frame (300ms) recovery with no input accepted, 1 HP damage is dealt, and the jump buffer window opens on frame 12 with execution on frame 18.

**AC-1.18 (CR-12b: Slope Handling)**
GIVEN the creature is running at full speed on dry stone in CALM context, WHEN the creature encounters a slope between 46 and 60 degrees, THEN uphill speed is reduced by 50% and downhill speed is increased by 25% compared to flat ground speed.

**AC-1.19 (CR-12b: Steep Slope Rejection)**
GIVEN the creature is approaching a surface angled above 60 degrees from horizontal, WHEN the creature makes contact, THEN the surface is treated as a wall (not traversable ground), and wall-slide activates if the surface is wall-interactive.

**AC-1.20 (CR-12e: Head Bonking)**
GIVEN the creature is ascending after a jump, WHEN the creature's collider contacts a ceiling, THEN vertical velocity is immediately set to 0 (upward movement stops), the creature transitions to Falling, and no stun or damage is applied.

**AC-1.21 (F-1: Effective Ground Speed Calculation)**
GIVEN the creature has a base run speed of 120 px/s, no evolution bonus, TENSE feel context (multiplier 1.17), on dry stone (accel mult 1.0), with full input (magnitude 1.0), WHEN effective ground speed is calculated, THEN the result is exactly 140.4 px/s ((120 + 0) * 1.17 * 1.0 * 1.0).

**AC-1.22 (F-2: Ground Deceleration Calculation)**
GIVEN the creature is running on wet stone (friction mult 0.6) in PANIC context (decel mult 0.75), WHEN the player releases input and deceleration is calculated, THEN effective deceleration is exactly 405.0 px/s^2 (900 * 0.75 * 0.6).

**AC-1.23 (F-3: Full Jump Height Calculation)**
GIVEN CALM context with no evolution bonuses (impulse = 280, ascending gravity = 800), WHEN the player holds jump to apex and height is calculated via impulse^2 / (2 * gravity), THEN the result is exactly 49.0 px (78400 / 1600).

**AC-1.24 (F-8: Landing Recovery Classification)**
GIVEN a fall duration of 0.5 seconds and an impact velocity of 350 px/s, WHEN the landing recovery tier is computed, THEN the tier is HARD (because while 0.5s <= 0.6s meets the MEDIUM duration check, 350 px/s exceeds the MEDIUM velocity ceiling of 300 px/s, pushing to HARD).

**AC-1.25 (F-9: Air Control Calculation)**
GIVEN TENSE context (air control ratio 0.75, accel mult 1.25), WHEN air acceleration is calculated, THEN the result is exactly 562.5 px/s^2 (600 * 0.75 * 1.25).

**AC-1.26 (F-13: Slope Speed Calculation)**
GIVEN the creature is running at an effective ground speed of 120 px/s on a 50-degree uphill slope, WHEN slope speed is calculated, THEN the result is exactly 60 px/s (120 * 0.5 uphill modifier).

**AC-1.27 (F-17: Descending Velocity at Impact)**
GIVEN the creature falls from the apex with descending gravity 1000 px/s^2 and default terminal velocity of 350 px/s, WHEN 0.2 seconds of falling have elapsed, THEN impact velocity is exactly 200 px/s (1000 * 0.2), and WHEN 0.35 seconds or more have elapsed, THEN velocity is capped at 350 px/s.

---

### 2. State Machine (Transitions, Priorities, Entry/Exit Actions)

**AC-2.1 (State Priority: DEAD Over All)**
GIVEN the creature is in any state (including STUNNED, SWIMMING, SQUEEZING, or any other), WHEN health reaches 0 or below, THEN the creature immediately transitions to DEAD (priority 100), overriding all other active transitions on that frame.

**AC-2.2 (State Priority: STUN Over Movement)**
GIVEN the creature is in any state except DEAD, WHEN an external force exceeding 200 px/s magnitude is applied, THEN the creature immediately transitions to STUNNED (priority 99), overriding all lower-priority transitions.

**AC-2.3 (State Priority: CRAWLING Over JUMPING)**
GIVEN the creature is standing on a floor, WHEN the player presses Jump and Down on the same frame, THEN CRAWLING (priority 90) wins over JUMPING (priority 80), the creature enters crawl, and the jump input enters the 6-frame buffer.

**AC-2.4 (State Transition: IDLE to WALKING)**
GIVEN the creature is in IDLE state on a floor, WHEN horizontal input with magnitude below 0.4 is applied, THEN the creature transitions to WALKING (transition 50, priority 40).

**AC-2.5 (State Transition: WALKING to RUNNING)**
GIVEN the creature is in WALKING state, WHEN horizontal input magnitude increases to >= 0.4, THEN the creature transitions to RUNNING (transition 52, priority 38) with no transition animation -- speed simply increases.

**AC-2.6 (State Transition: RUNNING to IDLE)**
GIVEN the creature is in RUNNING state, WHEN horizontal input is released AND velocity drops below 5 px/s, THEN the creature transitions to IDLE (transition 55, priority 35). The 5 px/s threshold prevents premature idle during slow deceleration.

**AC-2.7 (State Entry: JUMPING)**
GIVEN the creature transitions into JUMPING state, WHEN the entry action executes, THEN jump impulse is applied as a single-frame velocity change, the coyote timer is consumed, and the jump buffer is consumed.

**AC-2.8 (State Entry: WALL_SLIDING)**
GIVEN the creature transitions into WALL_SLIDING state, WHEN the entry action executes, THEN `wall_slide_timer` is set to 0 and the wall direction (left/right) is recorded.

**AC-2.9 (State Entry: DEAD)**
GIVEN the creature transitions into DEAD state, WHEN the entry action executes, THEN all velocity is zeroed, collision is disabled, the `died` signal is emitted, a death animation plays for 30 frames (500ms), and then `request_respawn` signal is emitted.

**AC-2.10 (State Exit: CLIMBING)**
GIVEN the creature is in CLIMBING state, WHEN the creature transitions out of CLIMBING to any other state, THEN normal gravity is restored (from the 0 gravity used during climbing).

**AC-2.11 (State Exit: CRAWLING)**
GIVEN the creature is in CRAWLING state, WHEN the creature transitions to a standing state (IDLE, WALKING, RUNNING), THEN the collider reshapes back from 12x6 to 10x12 over 4 frames.

**AC-2.12 (Coyote Time Management)**
GIVEN the creature walks or runs off a ledge (not jumping, not pushed), WHEN `is_on_floor()` becomes false, THEN a coyote timer counting down from 6 frames begins, and pressing jump before the timer expires produces a ground jump with full ground-jump impulse.

**AC-2.13 (Jump Buffer Management)**
GIVEN the player presses jump while not on a jumpable surface, WHEN the creature contacts a floor or wall within 6 frames, THEN the appropriate jump (ground or wall) executes on the frame of contact.

**AC-2.14 (Implicit Transition: WALL_JUMPING to Ground)**
GIVEN the creature is in WALL_JUMPING state, WHEN `is_on_floor()` becomes true before the 8-frame arc expires, THEN the creature transitions to IDLE/WALKING/RUNNING based on landing recovery logic from CR-11, and the wall-jump input lockout timer is cleared.

---

### 3. Feel Layer (Context Switching, Parameter Modification, Interpolation)

**AC-3.1 (CALM Context)**
GIVEN no predators are within the 160px awareness radius, no environmental hazards are active, and the player is in explored territory, WHEN the feel layer evaluates threat context, THEN the active context is CALM with all multipliers at 1.0 baseline (max run speed 1.0, ground acceleration 1.0, ground deceleration 1.0, jump impulse 1.0, turnaround skid 4 frames).

**AC-3.2 (TENSE Context)**
GIVEN a predator is detected within the awareness radius but not in active pursuit, WHEN the feel layer context is TENSE, THEN max run speed multiplier is 1.17, ground acceleration multiplier is 1.25, ground deceleration multiplier is 0.9, turnaround skid is 3 frames, and wall jump input lockout is 5 frames.

**AC-3.3 (PANIC Context)**
GIVEN a predator is in active pursuit (Creature AI has flagged pursuit state targeting the player), WHEN the feel layer context is PANIC, THEN max run speed multiplier is 1.33, ground acceleration multiplier is 1.5, ground deceleration multiplier is 0.75, air control ratio is 0.6 (reduced from 0.7 baseline), and turnaround skid is 2 frames.

**AC-3.4 (PRECISE Context)**
GIVEN the player is within a traversal challenge zone (flagged by level geometry), WHEN the feel layer context is PRECISE, THEN max run speed multiplier is 0.75, ground deceleration multiplier is 1.22, air control ratio is 0.85, and wall jump input lockout is 10 frames.

**AC-3.5 (SHELTERED Context)**
GIVEN the player is inside a shelter zone, WHEN the feel layer context is SHELTERED, THEN max run speed multiplier is 0.5, ground acceleration multiplier is 0.6, ground deceleration multiplier is 1.3, jump impulse multiplier is 0.85, and turnaround skid is 6 frames.

**AC-3.6 (Context Priority)**
GIVEN the creature is inside a shelter zone AND a predator is in active pursuit simultaneously, WHEN the feel layer evaluates context priority, THEN SHELTERED (highest priority) is active, not PANIC. Priority order is: SHELTERED > PANIC > PRECISE > TENSE > CALM.

**AC-3.7 (PANIC Priority Over PRECISE)**
GIVEN the creature is in a traversal challenge zone AND a predator is in active pursuit, WHEN the feel layer evaluates context priority, THEN PANIC is active (not PRECISE), because PANIC > PRECISE in the priority hierarchy.

**AC-3.8 (Transition TO PANIC is Instant)**
GIVEN the creature is in any non-PANIC context, WHEN threat context changes to PANIC, THEN all parameter multipliers snap to PANIC values on that exact frame with 0 interpolation frames.

**AC-3.9 (Transition FROM PANIC Lerps Over 20 Frames)**
GIVEN the creature is in PANIC context, WHEN threat context changes to any other context (e.g., CALM), THEN parameter multipliers lerp from PANIC values to the target values over exactly 20 frames (333ms). At frame 10 of 20 (halfway), each multiplier is exactly halfway between PANIC and target values.

**AC-3.10 (Default Transition Lerps Over 10 Frames)**
GIVEN the creature is in CALM context, WHEN threat context changes to TENSE, THEN parameter multipliers lerp from CALM to TENSE values over exactly 10 frames (167ms). At frame 4 of 10, max run speed multiplier is exactly 1.068 (1.0 + (1.17 - 1.0) * 0.4).

**AC-3.11 (Interrupted Lerp)**
GIVEN a CALM-to-TENSE lerp is in progress at frame 5 of 10, WHEN threat context changes to PANIC, THEN the lerp is immediately abandoned and PANIC values apply instantly (TO PANIC is always instant, regardless of any in-progress lerp).

**AC-3.12 (F-14: Feel Layer Interpolation Calculation)**
GIVEN a transition from PANIC (max run speed mult 1.33) to CALM (mult 1.0) over 20 frames, WHEN frame 10 of 20 is reached, THEN the current multiplier is exactly 1.165 (1.33 + (1.0 - 1.33) * 0.5).

---

### 4. Surface Interaction (Friction, Climbing, Material Differences)

**AC-4.1 (CR-7: Dry Stone Baseline)**
GIVEN the creature is running on dry stone, WHEN surface properties are read, THEN friction multiplier is 1.0, acceleration multiplier is 1.0, slide speed multiplier is 1.0, the surface is wall-interactive, and the surface is NOT climbable.

**AC-4.2 (CR-7: Ice Surface)**
GIVEN the creature is running on ice, WHEN surface properties are applied, THEN friction multiplier is 0.2 (deceleration is 20% effective), acceleration multiplier is 0.5 (acceleration halved), and the surface is neither wall-interactive nor climbable.

**AC-4.3 (CR-7: Wet Metal)**
GIVEN the creature is running on wet metal, WHEN surface properties are applied, THEN friction multiplier is 0.4 and stopping from 120 px/s takes approximately 0.333 seconds (120 / (900 * 0.4)) instead of the 0.133 seconds on dry stone.

**AC-4.4 (CR-7: Moss Surface)**
GIVEN the creature is running on moss, WHEN surface properties are applied, THEN friction multiplier is 1.3, wall slide speed multiplier is 0.8, stops are crisper than baseline, and the surface IS wall-interactive.

**AC-4.5 (CR-7: Crumbling Tile)**
GIVEN the creature is standing on a crumbling tile, WHEN 0.5 seconds of continuous contact have elapsed, THEN the tile breaks (collision removed), and after 4.0 seconds the tile respawns (collision restored).

**AC-4.6 (CR-7: Surface Detection at Tile Boundary)**
GIVEN the creature's collider straddles two different surface tiles at exactly 50/50 overlap, WHEN the active surface is determined, THEN the lower-friction surface wins (conservative for the player).

**AC-4.7 (CR-7: Surface Detection at >50% Overlap)**
GIVEN the creature's collider overlaps two surface tiles with more than 50% on one tile, WHEN the active surface is determined, THEN the tile with greater than 50% contact area is selected as the active surface.

**AC-4.8 (CR-7: Abrupt Surface Transition)**
GIVEN the creature is running on dry stone (friction 1.0) and the next tile ahead is ice (friction 0.2), WHEN the >50% overlap rule selects ice, THEN the new friction applies instantly on that frame with no blending or lerp between surface frictions.

**AC-4.9 (F-15: Surface Friction with Weather Override)**
GIVEN dry stone during rain (weather overrides friction to 0.6) and no evolution friction minimum, WHEN effective friction is calculated, THEN the result is exactly 0.6 (max(0.6, 0.0)).

**AC-4.10 (F-15: Surface Friction with Evolution Minimum)**
GIVEN ice surface (friction 0.2) and evolution hardened foot pads (friction minimum 0.8), WHEN effective friction is calculated, THEN the result is exactly 0.8 (max(0.2, 0.8)), transforming ice from nearly frictionless to manageable.

**AC-4.11 (F-16: Effective Climb Speed Calculation)**
GIVEN climbing wet vines (surface mult 0.7) in TENSE context (feel mult 1.3), WHEN effective climb speed is calculated, THEN the result is exactly 45.5 px/s (50 * 0.7 * 1.3).

---

### 5. Wall Mechanics (Slide, Cling, Jump, Fatigue)

**AC-5.1 (CR-4a: Wall Slide Initiation)**
GIVEN the creature is airborne and pressing horizontal input toward a wall-interactive surface, WHEN the creature contacts the wall, THEN the creature enters WALL_SLIDING state and fall speed is reduced to a maximum of 80 px/s (from terminal velocity of 350 px/s) in CALM context.

**AC-5.2 (CR-4a: Wall Slide on Non-Interactive Surface)**
GIVEN the creature is airborne and pressing toward a smooth metal surface (not wall-interactive), WHEN the creature contacts the wall, THEN the creature does NOT enter wall-slide and continues falling at normal speed.

**AC-5.3 (CR-4a: Wall Slide Duration Limit)**
GIVEN the creature has been wall-sliding on the same wall for 2.0 seconds in CALM context with no evolution bonuses, WHEN the timer exceeds 2.0 seconds, THEN slide speed increases by 50 px/s per second above the base slide speed, accelerating toward terminal velocity.

**AC-5.4 (CR-4b: Wall Cling)**
GIVEN the creature is in WALL_SLIDING state on a surface with wall_cling_allowed flag, WHEN the player activates the grab input, THEN vertical velocity drops to 0, the creature holds position on the wall, and the creature remains stationary until input changes.

**AC-5.5 (CR-4b: Wall Cling on Non-Cling Surface)**
GIVEN the creature is in WALL_SLIDING state on a surface WITHOUT wall_cling_allowed (e.g., smooth stone that allows slide but not cling), WHEN the player activates the grab input, THEN the cling does NOT activate and the creature continues wall-sliding.

**AC-5.6 (CR-4c: Wall Jump Impulse)**
GIVEN the creature is in WALL_SLIDING or WALL_CLINGING state (first wall jump, no fatigue), WHEN the player presses jump, THEN the creature receives a vertical impulse of -260 px/s (upward) and a horizontal impulse of 160 px/s (away from wall).

**AC-5.7 (CR-4c: Wall Jump Input Lockout)**
GIVEN the creature has just executed a wall jump in CALM context, WHEN the first 8 frames (133ms) after the wall jump are examined, THEN horizontal input toward the wall is suppressed (no re-grab allowed). After 8 frames, full air control resumes.

**AC-5.8 (CR-4c: Wall Jump Fatigue)**
GIVEN the creature executes consecutive wall jumps on the same wall without touching ground or a different wall, WHEN the vertical impulse of each jump is measured, THEN: 1st jump = -260 px/s, 2nd = -208 px/s (20% reduction), 3rd = -166 px/s (40% reduction), 4th and beyond = -133 px/s (clamped floor).

**AC-5.9 (CR-4c: Wall Jump Fatigue Reset)**
GIVEN the creature has wall-jump fatigue of 3 consecutive jumps on the same wall, WHEN the creature touches the ground or contacts a different wall, THEN the fatigue counter resets to 0 and the next wall jump uses full -260 px/s impulse.

**AC-5.10 (F-6: Wall Jump Fatigue Calculation)**
GIVEN the third consecutive wall jump on the same wall (n=2) with base fatigue rate 0.2 and no evolution reduction, WHEN wall jump vertical impulse is calculated, THEN the result is exactly 156.0 px/s (260 * (1.0 - 0.2 * 2) = 260 * 0.6). This is above the floor of 133, so it is not clamped.

**AC-5.11 (F-6: Wall Jump Fatigue Floor Clamping)**
GIVEN the fourth consecutive wall jump on the same wall (n=3) with base fatigue rate 0.2, WHEN wall jump vertical impulse is calculated, THEN 260 * (1.0 - 0.2 * 3) = 260 * 0.4 = 104.0 px/s, which is below the floor of 133, so the impulse is clamped to exactly 133.0 px/s.

**AC-5.12 (F-7: Wall Slide Grip Degradation Calculation)**
GIVEN wall-sliding on dry stone in CALM for 3.5 seconds with no evolution bonus (grip_duration = 2.0), WHEN wall slide speed is calculated, THEN the result is exactly 155.0 px/s (80.0 + 50.0 * (3.5 - 2.0)).

---

### 6. Water Mechanics (Swim, Oxygen, Submerge)

**AC-6.1 (CR-10: Automatic Water Entry)**
GIVEN the creature is grounded or airborne, WHEN the creature's detector area overlaps with a water volume Area2D, THEN the creature automatically transitions to SWIMMING_SURFACE (priority 98) without any player input required.

**AC-6.2 (CR-10: Surface Swimming Speed)**
GIVEN the creature is surface swimming in CALM context with no evolution bonuses, WHEN the player provides full horizontal input, THEN the creature moves at 70 px/s horizontally with acceleration of 350 px/s^2 and deceleration of 500 px/s^2.

**AC-6.3 (CR-10: Surface Swimming Position)**
GIVEN the creature is in SWIMMING_SURFACE state, WHEN the creature's visual position is inspected, THEN the top 30% of the sprite is above the water surface line and the bottom 70% is below.

**AC-6.4 (CR-10: Submerge Requires Evolution)**
GIVEN the creature is in SWIMMING_SURFACE state with default water_movement_mode of SURFACE, WHEN the player presses Down, THEN the creature does NOT submerge and bobs back up at the surface.

**AC-6.5 (CR-10: Submerge With Evolution)**
GIVEN the creature is in SWIMMING_SURFACE state with water_movement_mode set to SUBMERGED or AMPHIBIOUS via evolution, WHEN the player presses Down, THEN the creature transitions to SWIMMING_SUBMERGED and enters full 2D underwater movement.

**AC-6.6 (CR-10: Water Surface Jump)**
GIVEN the creature is in SWIMMING_SURFACE state, WHEN the player presses jump, THEN the creature receives a vertical impulse of -200 px/s (weaker than the ground jump impulse of -280 px/s).

**AC-6.7 (CR-10: Oxygen Depletion)**
GIVEN the creature is in SWIMMING_SUBMERGED state with no evolution bonuses, WHEN 5.0 seconds of submersion have elapsed, THEN oxygen is fully depleted and the creature takes 1 HP damage per second until surfacing, exiting water, or dying.

**AC-6.8 (CR-10: Water Exit)**
GIVEN the creature is surface swimming adjacent to a ground surface, WHEN the player inputs horizontal movement toward the ground edge, THEN the creature performs an auto-climb-out animation over 6 frames (100ms) and transitions to IDLE on the ground.

**AC-6.9 (F-10: Effective Swim Speed Calculation)**
GIVEN surface swimming in TENSE context with gill-lungs evo granting +15 swim bonus, WHEN effective swim speed is calculated, THEN the result is exactly 102.0 px/s ((70 + 15) * 1.2).

**AC-6.10 (F-11: Oxygen Duration with Evolution)**
GIVEN the creature has gill-lungs evolution (swim_oxygen_multiplier = 3.0), WHEN effective oxygen duration is calculated, THEN the result is exactly 15.0 seconds (5.0 * 3.0), and at 12.0 seconds submerged the creature still has 3.0 seconds of oxygen remaining.

---

### 7. Crawl/Squeeze (Collider Reshaping, Forced Zones)

**AC-7.1 (CR-8: Crawl Initiation)**
GIVEN the creature is in IDLE, WALKING, or RUNNING state on a floor, WHEN the player presses Down, THEN the creature enters CRAWLING state and the collider reshapes from 10x12 to 12x6 over exactly 4 frames (67ms).

**AC-7.2 (CR-8: Crawl Movement Speed)**
GIVEN the creature is in CRAWLING state, WHEN the player provides horizontal input, THEN the creature moves at a maximum speed of 40 px/s with acceleration of 300 px/s^2 and deceleration of 600 px/s^2.

**AC-7.3 (CR-8: No Jump While Crawling)**
GIVEN the creature is in CRAWLING state with a ceiling above preventing standing, WHEN the player presses jump on a normal (non-one-way) surface, THEN no jump occurs. The creature must stand first, which requires vertical clearance for the standing collider.

**AC-7.4 (CR-8: Forced Crawl Zone)**
GIVEN the creature is crawling in a passage with a ceiling lower than the standing collider height (12px), WHEN the player releases the Down input, THEN the ShapeCast for the standing collider fails and the creature remains in CRAWLING state until the passage opens.

**AC-7.5 (CR-8b: Squeeze Initiation)**
GIVEN the creature is in CRAWLING state, WHEN the creature enters a passage narrower than the 12px crawl collider width, THEN the collider compresses from 12x6 to 8x4 over exactly 6 frames (100ms) and speed drops to 20 px/s.

**AC-7.6 (CR-8b: Squeeze Movement Restrictions)**
GIVEN the creature is in SQUEEZING state, WHEN the player provides input, THEN the creature can only move forward (direction it entered) or backward at exactly 20 px/s with instant deceleration (0 frames to stop). No vertical movement and no jumping are possible.

**AC-7.7 (CR-8b: Squeeze Reversal)**
GIVEN the creature is in SQUEEZING state moving forward, WHEN the player presses the opposite horizontal input, THEN the creature pauses for 4 frames (67ms turnaround) and then moves backward at squeeze speed (20 px/s).

**AC-7.8 (CR-8b: Squeeze Forces PRECISE Context)**
GIVEN the creature is in SQUEEZING state and a predator is in active pursuit (would normally trigger PANIC), WHEN the feel layer evaluates context, THEN PRECISE context is active (not PANIC), and the creature cannot squeeze faster regardless of external threats.

**AC-7.9 (Collider Reshape Interruption -- EC-3)**
GIVEN the creature is mid-reshape from crawl to standing (12x6 to 10x12) and a ceiling moves downward removing clearance, WHEN the ShapeCast on the current reshape frame detects collision, THEN the reshape halts at the current intermediate dimensions and resumes when clearance returns (without restarting from frame 0).

**AC-7.10 (Crawl on One-Way Platform -- EC-8)**
GIVEN the creature is CRAWLING on a one-way platform (Down is held), WHEN the player presses Jump, THEN a drop-through executes (collision disabled for 6 frames), the creature falls through with the crawl collider active, and transitions to FALLING in crawl state.

---

### 8. Evolution Interface (Hook Application, Mid-State Mutations)

**AC-8.1 (CR-9: Default Hook Values)**
GIVEN no evolution mutations have been acquired, WHEN the Player Controller reads the evolution hook table, THEN all hooks return their default values: collider_dimensions = (10, 12), max_run_speed_bonus = 0.0, jump_impulse_bonus = 0.0, gravity_multiplier = 1.0, terminal_velocity_override = -1.0, swim_oxygen_multiplier = 1.0, surface_friction_minimum = 0.0, wall_interactive_override = [], can_wall_cling = true.

**AC-8.2 (CR-9: Evolution Speed Bonus Application)**
GIVEN evolution grants max_run_speed_bonus = 30.0, WHEN the creature runs on dry stone in CALM context, THEN the effective max speed is 150 px/s ((120 + 30) * 1.0 * 1.0).

**AC-8.3 (CR-9: Gliding Membrane)**
GIVEN evolution sets gravity_multiplier to 0.5 and terminal_velocity_override to 150 px/s, WHEN the creature is falling, THEN ascending gravity is 400 px/s^2 (800 * 0.5), descending gravity is 500 px/s^2 (1000 * 0.5), and terminal velocity is 150 px/s instead of 350.

**AC-8.4 (CR-9: Gecko Pads)**
GIVEN evolution adds "metal_dry" and "metal_wet" to wall_interactive_override, WHEN the creature is airborne and presses into a dry or wet metal wall, THEN wall-slide activates on those surfaces (normally non-wall-interactive).

**AC-8.5 (CR-9: Invalid Hook Clamping)**
GIVEN evolution writes a negative value for collider_dimensions (e.g., (-5, 12)), WHEN the Player Controller reads the hook table, THEN the value is clamped to the valid range (minimum 4px per dimension per hard cap) and a warning signal is emitted.

**AC-8.6 (EC-5: Gravity Change Mid-Jump)**
GIVEN the creature is mid-jump with ascending gravity of 800 px/s^2, WHEN evolution changes gravity_multiplier to 0.5, THEN ascending gravity drops to 400 px/s^2 on the next physics frame and the creature floats higher than the original arc with no compensation or snap correction.

**AC-8.7 (EC-5: Terminal Velocity Change Mid-Fall)**
GIVEN the creature is falling at 300 px/s and evolution activates gliding (terminal_velocity_override = 150), WHEN the new terminal velocity is applied, THEN the creature does NOT snap to 150 px/s instantly. Instead, the creature decelerates toward 150 px/s at 500 px/s^2 (descending gravity * 0.5) over approximately 0.3 seconds.

**AC-8.8 (EC-5: Max Speed Change While Running)**
GIVEN the creature is running at 150 px/s (with an evolution bonus), WHEN evolution removes the speed bonus (new max becomes 120 px/s), THEN the creature does not instantly lose speed. Speed exceeding the new max by up to 10% coasts via normal deceleration, and acceleration no longer increases speed beyond the new max.

**AC-8.9 (EC-5: Can_wall_cling Disabled Mid-Cling)**
GIVEN the creature is in WALL_CLINGING state, WHEN evolution sets can_wall_cling to false, THEN on the next frame the creature is forced into WALL_SLIDING (equivalent to releasing grab input).

**AC-8.10 (EC-3: Collider Growth in Tight Space)**
GIVEN evolution grows the standing collider from (10, 12) to (14, 16) and the creature is in a space that does not fit the new dimensions, WHEN the mutation_applied signal fires, THEN the controller performs a ShapeCast, finds no fit, searches 4 cardinal directions within 8px for a valid position, and either micro-teleports or defers the collider change via pending_collider_update.

**AC-8.11 (EC-3: Evolution During Squeeze)**
GIVEN the creature is in SQUEEZING state, WHEN evolution attempts to change squeeze_collider_dimensions, THEN the change is deferred (stored in pending_collider_update) and applied only when the creature transitions out of SQUEEZING state.

---

### 9. System Interactions (Each INT Contract Verified)

**AC-9.1 (INT-1: Input System)**
GIVEN the Input System is providing device-agnostic action data, WHEN the Player Controller processes input, THEN it reads ONLY from action-mapped values (move_axis, jump_pressed, jump_held, jump_released, grab_pressed, grab_held, grab_released, down_pressed, down_held) and NEVER reads raw device input via `Input.is_key_pressed()` or equivalent.

**AC-9.2 (INT-2: Camera System)**
GIVEN the Camera System reads Player Controller properties every frame, WHEN any Player Controller property (global_position, velocity, current_state, facing_direction, threat_context) changes, THEN the Camera System reads the updated value, and the Player Controller NEVER calls any Camera method or reads any Camera state.

**AC-9.3 (INT-3: Biological Evolution -- Write Direction)**
GIVEN the Evolution system acquires a mutation, WHEN it writes to the hook table (e.g., sets gravity_multiplier to 0.5), THEN the Player Controller reads the new value on the next physics frame. The Evolution system NEVER modifies the Player Controller's velocity, state, or position directly.

**AC-9.4 (INT-3: Biological Evolution -- Read Direction)**
GIVEN the Player Controller is running, WHEN the Evolution system queries the controller's state, THEN it can read current_state, current_surface, time_in_state, cumulative_biome_exposure, damage_sources, and is_alive. The Player Controller NEVER writes to the evolution hook table.

**AC-9.5 (INT-4: Shelter & Rest -- Respawn)**
GIVEN the creature has died and the 30-frame death animation completes, WHEN `request_respawn` is emitted, THEN the Shelter & Rest system calls `respawn(position, state)` on the controller, which sets position to last_shelter_position, resets velocity to zero, resets state to IDLE, applies respawn_state values (full health, full oxygen), and enables collision.

**AC-9.6 (INT-4: Shelter & Rest -- SHELTERED Context)**
GIVEN the creature enters a shelter zone, WHEN the Shelter & Rest system sets is_in_shelter to true, THEN the Player Controller's feel layer reads this value and activates SHELTERED context (highest priority in the feel layer).

**AC-9.7 (INT-5: Diegetic UI -- Read Only)**
GIVEN the Diegetic UI system reads health_current, health_max, oxygen_current, current_state, threat_context, and velocity from the Player Controller, WHEN the UI processes these values, THEN the UI NEVER calls any Player Controller method or modifies any Player Controller property. All communication is strictly read-only.

**AC-9.8 (INT-6: Player Visual System)**
GIVEN the Player Visual system reads current_state, velocity, facing_direction, and threat_context, WHEN the visual system plays an animation, THEN animation playback does NOT affect controller state. The collider shape is owned by the controller, not the visual system. The sprite may extend beyond the collider (16x16 sprite vs 10x12 collider).

**AC-9.9 (INT-7: World/Biome -- Threat Context)**
GIVEN the World/Biome system aggregates data from Creature AI, Environmental Hazards, and zone flags, WHEN it produces a threat_context enum value, THEN the Player Controller reads this single enum value and does NOT read individual creature states, predator positions, or raw hazard data. The controller responds only to the aggregated context.

**AC-9.10 (INT-8: Time & Weather -- Indirect)**
GIVEN rain is active and the Time & Weather system communicates this to the World/Biome system, WHEN the World/Biome system provides weather_surface_modifier to the Player Controller, THEN the controller applies the friction override (e.g., dry stone becomes wet stone friction of 0.6) without any direct dependency on the Time & Weather system.

**AC-9.11 (INT-9: Creature AI -- Decoupled)**
GIVEN a Creature AI entity is in active pursuit of the player, WHEN the Creature AI reads the player's global_position, velocity, and current_state, THEN it does so via the player node reference (not a dedicated interface), and physical interactions (knockback, pushing) are handled as external forces applied via CR-12d through Godot's physics.

**AC-9.12 (INT-10: Environmental Hazards -- Damage)**
GIVEN the creature is in a hazard zone, WHEN the Environmental Hazards system calls damage(amount, type) on the Player Controller, THEN the controller subtracts from health, checks for death, and emits `damage_taken(amount, type)` signal. The controller does NOT own hazard logic -- it applies the damage it receives without questioning it.

**AC-9.13 (INT-11: Narrative Discovery -- Speed Reduction)**
GIVEN the creature is near a Narrative Discovery node and the player has engaged interaction, WHEN discovery_interaction_active is true, THEN the Player Controller applies a 50% max speed reduction. The creature does NOT stop moving, enter a cutscene, or lose movement control.

---

### 10. Performance (Frame Budget, Physics Consistency)

**AC-10.1 (Frame Budget)**
GIVEN the game is running at 480x270 viewport resolution with the player creature active, WHEN the Player Controller's `_physics_process(delta)` executes, THEN the controller completes all computation (state evaluation, physics application, surface detection, feel layer calculation, and `move_and_slide()`) within 2ms or less, leaving the remaining 14.6ms of the 16.67ms frame budget for other systems.

**AC-10.2 (Physics Step Consistency)**
GIVEN the physics tick rate is set to 60fps, WHEN 1000 physics frames are measured over a 10-second window, THEN no individual frame's delta exceeds 0.020s (20ms) and no frame's delta falls below 0.014s (14ms), maintaining consistent physics simulation.

**AC-10.3 (Deterministic Transitions)**
GIVEN the same input sequence and initial state, WHEN the same sequence is replayed twice on the same frame timing, THEN the creature follows the identical path and state transitions both times. No random seeds, no frame-timing variance in transition logic.

**AC-10.4 (Sub-Pixel Movement Accumulation)**
GIVEN the creature is moving at 0.5 px/s (extremely slow, e.g., SHELTERED with stacked slowdowns), WHEN 120 frames (2 seconds) elapse, THEN the creature has moved exactly 1 pixel total. Sub-pixel movement is accumulated as a float and `move_and_slide()` processes fractional pixel movement correctly.

**AC-10.5 (Hard Cap Enforcement)**
GIVEN extreme evolution stacking produces a max_run_speed_bonus of 200 and gravity_multiplier of 0.1, WHEN the Player Controller applies these values, THEN absolute hard caps are enforced: horizontal speed capped at 500 px/s, vertical speed capped at 600 px/s, gravity_multiplier clamped to minimum 0.05. A `parameter_clamped` warning signal is emitted in debug builds.

**AC-10.6 (Zero/Negative Parameter Clamping)**
GIVEN feel layer multipliers and evolution modifiers combine to produce a parameter value of 0 or negative, WHEN the resulting value is applied, THEN it is clamped to 0.01 (avoiding division-by-zero) and a `parameter_clamped` signal is emitted. Negative speed is never allowed.

---

### 11. Accessibility (Buffer Windows, Ledge Correction, Remapping)

**AC-11.1 (CR-12a: Ledge Assist)**
GIVEN the creature is jumping toward a platform edge and would miss the landing by 2px or fewer horizontally, WHEN the controller evaluates landing, THEN the creature's position is nudged by up to 2px to land on the platform. This nudge is invisible to the player and applies only to top corners of platform edges, not walls.

**AC-11.2 (CR-3a: Coyote Time Window)**
GIVEN the creature walks off a ledge (not jumping, not pushed), WHEN the player presses jump within 6 frames (100ms) of leaving the floor, THEN a full ground jump executes at the creature's current airborne position, even though the creature is visibly past the ledge.

**AC-11.3 (CR-3a: Coyote Time NOT After Wall Release)**
GIVEN the creature detaches from a wall (releases wall-slide or wall-cling), WHEN the creature enters FALLING, THEN no coyote time is granted. Coyote time activates ONLY after walking or running off a ledge.

**AC-11.4 (CR-3b: Jump Buffer)**
GIVEN the player presses jump 3 frames before the creature lands on a floor, WHEN the creature contacts the floor on the 3rd frame, THEN the jump executes immediately on the landing frame without the player needing to press jump again.

**AC-11.5 (F-4: Coyote Time with Evolution Bonus)**
GIVEN evolution grants coyote_time_bonus_frames = 3, WHEN coyote time is calculated, THEN the window is 9 frames (150ms) instead of the base 6 frames. The bonus is additive.

**AC-11.6 (Coyote Time Hard Cap)**
GIVEN evolution grants coyote_time_bonus_frames = 15, WHEN coyote time is calculated, THEN the total is clamped at the hard cap of 18 frames (300ms) regardless of the bonus (base 6 + bonus 15 = 21, clamped to 18).

**AC-11.7 (F-5: Jump Buffer Expiry on Landing Frame)**
GIVEN the jump buffer countdown reaches 0 on the exact frame the creature lands, WHEN the buffer is evaluated (decrement first, then check), THEN the buffer has expired and is NOT consumed. The creature lands without jumping. This is deterministic: decrement before check.

**AC-11.8 (Jump Buffer Consumed by Wall)**
GIVEN the jump buffer is active and the creature contacts a wall-interactive surface before contacting a floor, WHEN the wall contact occurs, THEN the buffer is consumed by the wall contact and a wall jump executes. The buffer does not distinguish between ground and wall targets.

**AC-11.9 (Input Remapping)**
GIVEN the Player Controller reads all input through the Input System's action-mapped interface (INT-1), WHEN the player remaps a button (e.g., jump from A to B on gamepad), THEN the Player Controller continues to function identically because it reads action names (jump_pressed), not device-specific buttons.

**AC-11.10 (Analog Walk/Run Blend)**
GIVEN a gamepad provides analog stick input between 0.0 and 1.0, WHEN the input magnitude is below 0.4 the creature walks, and when the magnitude is at or above 0.4 the creature runs, THEN the walk/run distinction is a continuous blend driven by analog magnitude, not a binary toggle, allowing fine-grained speed control.

**AC-11.11 (Extended Buffer/Coyote -- Tuning Knobs)**
GIVEN the extended_buffer_frames tuning knob is set to 12 and extended_coyote_frames is set to 10 in the accessibility settings, WHEN these values are active, THEN the jump buffer window is 12 frames (200ms) and the coyote time window is 10 frames (167ms), providing more generous timing for players who need it.

**AC-11.12 (CR-12c: Moving Platform Momentum)**
GIVEN the creature is standing on a moving platform traveling right at 40 px/s and the creature is running right at 100 px/s, WHEN the creature jumps, THEN the horizontal velocity on the launch frame is 140 px/s (creature's 100 + platform's 40), exceeding normal max run speed. This is intentional momentum preservation.

**AC-11.13 (F-18: Moving Platform Momentum Calculation)**
GIVEN creature jump impulse of (100, -280) px/s and platform velocity of (40, 0) px/s, WHEN jump_velocity is calculated, THEN the result is exactly (140, -280) px/s, with the combined horizontal velocity not capped by normal max run speed.

**AC-11.14 (CR-12d: External Force Stun Threshold)**
GIVEN an external force of exactly 200.0 px/s magnitude is applied, WHEN the stun check evaluates, THEN stun does NOT trigger (the check is strict greater-than: > 200, not >= 200). A force of 200.01 px/s DOES trigger stun.

**AC-11.15 (EC-10: Death and Respawn Cycle)**
GIVEN the creature's health reaches 0, WHEN the death sequence executes, THEN: velocity is zeroed, collision is disabled, `died` signal emits, death animation plays for 30 frames (500ms), `request_respawn` emits, the Shelter & Rest system provides respawn coordinates, the creature appears at last_shelter_position in IDLE state with full health, full oxygen, and collision enabled. Evolution hooks are preserved across death.

**AC-11.16 (EC-10: Damage During Death)**
GIVEN the creature is already in DEAD state, WHEN additional damage(amount, type) calls are made by other systems, THEN the calls return immediately with no effect. Health is not decremented below 0. No additional damage_taken signals are emitted. The death animation is not interrupted.

## Open Questions

| # | Question | Owner | Target Resolution | Impact if Unresolved |
|---|----------|-------|-------------------|---------------------|
| OQ-1 | **Godot 4.6 CharacterBody2D API changes**: The LLM knowledge cutoff is May 2025. Godot 4.4-4.6 may have changed `move_and_slide()` behavior, floor snap, or one-way platform handling. Verify all CharacterBody2D assumptions against 4.6 docs before implementation. | Technical Director | Before `/create-architecture` | Physics behavior may not match spec. HIGH risk. |
| OQ-2 | **Jolt physics engine for 2D platformers**: Godot 4.6 defaults to Jolt for physics. Jolt behavior for 2D CharacterBody2D (collision response, floor detection, slope handling) may differ from the old GodotPhysics engine. Verify Jolt compatibility with the movement spec. | Technical Director | Before implementation sprint | Movement feel may require retuning for Jolt. MEDIUM risk. |
| OQ-3 | **Evolution collider growth and level design**: If the creature grows from 10x12 to 18x20 collider via evolution, some passages designed for the base creature will become impassable. Is this intentional gating (evolution paths have physical consequences) or a soft-lock risk? | Game Designer / Bio Evolution GDD | During Biological Evolution GDD authoring | Potential soft-lock if critical paths require base-size collider. HIGH risk. |
| OQ-4 | **Feel layer source data**: The spec assumes World/Biome aggregates `threat_context` from Creature AI and Environmental Hazards. The exact aggregation logic (how proximity, pursuit state, and hazard severity map to the 5 context levels) is undefined here — it belongs in the World/Biome GDD. | World/Biome GDD author | During World/Biome GDD authoring | Feel layer may not trigger at the right moments. MEDIUM risk. |
| OQ-5 | **Swimming evolution gating**: The spec gates submerged swimming behind the `water_movement_mode` evolution hook. What happens in the MVP where only one evolution branch exists? If that branch doesn't include aquatic mutations, are all submerged water areas inaccessible in the MVP? | Game Designer | During Biological Evolution GDD and MVP level design | MVP level design may need to avoid deep water or provide alternate paths. MEDIUM risk. |
| OQ-6 | **External force stun duration**: The stun duration for external forces is "specified by the source system" (CR-12d). No default stun duration exists in this GDD. Each source system (Creature AI knockback, Environmental Hazard push, weather wind) must define its own stun duration. Is there a global default? | Systems Designer | During `/create-architecture` | Inconsistent stun durations across systems. LOW risk. |
| OQ-7 | **Animation budget feasibility**: VAR-1 specifies 244 unique frames at base size across all states and contexts. With evolution variants and health degradation tiers, total frame count could reach 400+. Is this achievable for a solo dev? Should priority tiers be defined for animation production? | Art Director | Before art production begins | Animation backlog may bottleneck production. MEDIUM risk. |
| OQ-8 | **Keyboard walk modifier discoverability**: The keyboard profile uses a walk modifier key (Shift/Ctrl) for fine speed control. With no tutorials and no button prompts (Anti-pillar: NOT hand-holdy), how does the keyboard player discover this? Is it in the key rebinding menu only? | UX Designer | During Settings & Accessibility GDD | Keyboard players may never discover walk, reducing their control expression. LOW risk. |
