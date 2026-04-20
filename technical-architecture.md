# Technical Architecture Document

## 1. Engine and Stack

### Core Technology

| Component | Choice | Rationale |
|-----------|--------|-----------|
| Engine | Godot 4.6 | Open source, lightweight, native 2D pipeline, GDScript integration |
| Primary Language | GDScript | Rapid iteration, engine-native, sufficient performance for gameplay logic |
| Performance Extensions | C# / GDExtension | Reserved for hot-path AI simulation, pathfinding, population math |
| Physics | Jolt (Godot 4.6 default) | Deterministic, performant rigid body and area detection |
| Testing | GdUnit4 | GDScript-native test framework, headless runner support |
| Version Control | Git | Trunk-based development, LFS for binary assets |

### Rendering Configuration

- **Native viewport:** 480x270 pixels (16:9, exactly 1/8 of 1080p).
- **Upscaling:** Integer scaling with nearest-neighbor filtering. At 1080p, this is a clean 4x scale. At 1440p, 5.33x with black bars or viewport stretch. At 4K, 8x clean.
- **Renderer:** Vulkan Forward+. The 2D workload is light enough that Forward+ overhead is negligible, and it keeps the door open for any 2D lighting features that benefit from it.
- **Frame budget:** 60fps target = 16.6ms per frame. Allocations:
  - Physics + collision: 2ms
  - AI simulation: 4ms
  - Rendering: 6ms
  - Audio: 1ms
  - Scripting/game logic: 2ms
  - Headroom: 1.6ms

---

## 2. Architecture Overview

### Scene Tree Structure

```
Root
+-- Game (Main scene, persistent)
    +-- WorldManager (Autoload reference, manages biome loading)
    +-- BiomeContainer (Current + adjacent biome scenes)
    |   +-- ActiveBiome
    |   |   +-- TileMapLayers (terrain, collision, decoration)
    |   |   +-- CreatureContainer (all active creature instances)
    |   |   +-- EnvironmentContainer (weather, lighting, particles)
    |   |   +-- RuinContainer (interactive ruin elements)
    |   |   +-- AudioContainer (spatial audio emitters)
    |   +-- AdjacentBiome_N (loaded, simplified simulation)
    |   +-- AdjacentBiome_S
    |   +-- AdjacentBiome_E
    |   +-- AdjacentBiome_W
    +-- PlayerCreature (persistent across biome transitions)
    +-- Camera2D (follows player, biome-aware boundaries)
    +-- UILayer (minimal: pause overlay, accessibility features only)
```

### Autoload Services

Singleton services that persist across scene transitions:

| Service | Responsibility |
|---------|---------------|
| `GameState` | Master game state: play mode, pause state, save/load orchestration, transition management |
| `EvolutionState` | Player creature's evolution tree, unlocked adaptations, current form data |
| `WorldState` | Persistent world flags, biome discovery status, narrative fragment collection, ecosystem population snapshots |
| `AudioManager` | Bus mixing, ambient layer management, spatial audio registration, mixing state transitions |
| `InputManager` | Input buffering, action mapping, device detection, analog processing |
| `ConfigManager` | User settings, accessibility options, display configuration, keybinds |

### Signal Architecture

Systems communicate through Godot's signal system to maintain loose coupling. No system directly references another system's internals.

**Signal flow principles:**
- Signals flow upward (children emit, parents connect) or through autoloads (any node emits to a service, service re-emits to subscribers).
- No signal chains longer than 3 hops. If a signal needs to traverse more than 3 connections, it routes through an autoload service.
- All gameplay-critical signals are defined in a `signals.gd` reference file for documentation. The actual signals live on the nodes that emit them.

**Key signal paths:**

```
Player takes damage -->
  PlayerCreature.health_changed -->
    EvolutionState.check_adaptation_triggers()
    AudioManager.update_player_audio_state()
    (Sprite degradation handled internally by PlayerCreature)

Creature dies -->
  Creature.died(species, position) -->
    WorldState.update_population(species, -1)
    ActiveBiome.handle_creature_death(position)
    AudioManager.remove_spatial_emitter(creature_id)

Biome transition -->
  WorldManager.biome_transition_started(from, to) -->
    GameState.begin_transition()
    WorldState.snapshot_biome(from)
    AudioManager.crossfade_biome_ambience(from, to)
  WorldManager.biome_transition_completed(to) -->
    GameState.end_transition()
```

### Resource-Based Configuration

No magic numbers in code. All tunable values live in Godot Resource files (`.tres`):

- **CreatureSpeciesData** (Resource) -- Per-species: base stats, behavioral weights, sensory ranges, vocalization references, sprite sheets.
- **BiomeData** (Resource) -- Per-biome: tile sets, creature population rules, weather probabilities, ambient audio references, parallax layer configurations.
- **EvolutionNodeData** (Resource) -- Per-evolution: stat modifications, new ability references, sprite changes, audio signature changes.
- **WeatherStateData** (Resource) -- Per-weather-state: audio layers, particle configurations, lighting changes, creature behavior modifiers.

Resources can be edited in Godot's inspector without touching code. Designers and artists can tune gameplay without programmer involvement.

---

## 3. Performance Architecture

### Simulation Level-of-Detail (LOD)

The world simulates beyond what the player can see, but not at full fidelity everywhere:

| Zone | Distance | Simulation Level | Update Rate |
|------|----------|------------------|-------------|
| **Active** | Current biome | Full: physics, pathfinding, sensory model, animation, audio | Every frame |
| **Adjacent** | Neighboring biomes (4 cardinal) | Reduced: statistical population model, simplified movement, no animation/audio | Every 10 frames |
| **Distant** | All other biomes | Minimal: population numbers only, event probability rolls | Every 60 frames (1/sec) |

**LOD transitions:** When the player approaches a biome boundary, the adjacent biome transitions from statistical to full simulation over a 2-second window. Creatures are spawned at statistically appropriate positions. The player should never see a biome "pop in" -- by the time it's visible, it's fully simulated.

### Entity Budgets

Hard limits enforced by the entity manager:

| Resource | Budget | Enforcement |
|----------|--------|-------------|
| Total sprites on screen | 64 | Entity manager culls lowest-priority off-screen entities first |
| Animated sprites | 24 | Distant entities freeze animation, use static frame |
| Particle emitters | 6 | Weather particles share emitters, creature effects are pooled |
| Visible tiles | 600 | Viewport size enforces this naturally at 480x270 |
| Draw calls | <120 | Texture atlasing, batched rendering, minimize material switches |
| VRAM ceiling | 128MB | Texture budget per biome, shared atlas for common elements |
| Audio voices | 32 | Priority system: player > predators > nearby creatures > ambient |

### Parallax System

Each biome has up to 5 parallax layers:

| Layer | Depth | Content | Scroll Rate |
|-------|-------|---------|-------------|
| 0 (Farthest) | Sky/horizon | Atmospheric color, distant silhouettes | 0.05x |
| 1 | Far background | Distant ruins, mountains, treeline | 0.15x |
| 2 | Mid background | Medium-distance structures, large vegetation | 0.35x |
| 3 | Near background | Close background details, behind-player elements | 0.65x |
| 4 (Foreground) | Overlay | Foreground vegetation, rain close-drops, dust | 1.2x |

Parallax layers use pre-rendered tile strips, not individual sprites, to minimize draw calls.

### Memory Management

- **Biome loading:** Biomes are loaded as packed scenes. Only current + adjacent biomes are in memory. Transitioning loads the new adjacent set and frees the old.
- **Texture atlasing:** Each biome has a master atlas (max 2048x2048). Creature sprites share a global creature atlas. UI elements (minimal) share a small utility atlas.
- **Object pooling:** Frequently spawned/despawned objects (particles, projectiles, small creatures) use object pools. Pool sizes are defined in BiomeData resources.
- **GC awareness:** Avoid allocations in hot loops. Pre-allocate arrays for pathfinding, sensory queries, and spatial hashing. Godot's GDScript GC is generational; keep per-frame allocations under 1KB.

---

## 4. Save System Architecture

### Three-Tier Save Model

Not all data is equally important to persist. The save system is divided into three tiers based on reconstruction cost:

**Tier 1: Always-Save (critical, irreplaceable)**
- Player evolution state (full tree, current form, unlocked adaptations)
- World flags (narrative fragments found, one-time events triggered, biome discovery)
- Biome population summaries (species counts, predator/prey ratios -- enough to reconstruct a statistically valid biome)
- Player position (biome + coordinates)
- Play time, settings

**Tier 2: Exit-Save (valuable, expensive to reconstruct)**
- Individual creature states in the active and adjacent biomes (position, health, behavioral state, current target)
- Weather state and progression
- Active environmental events (flooding, migration, etc.)
- Recent player death location and cause (for corpse/stain placement)

**Tier 3: Regenerated (cheap, procedurally reproducible)**
- Decorative tile variations
- Ambient creature placement in distant biomes
- Particle system states
- Non-interactive environmental details

### Save Triggers

- **Shelter save:** Entering a shelter triggers a full Tier 1 + Tier 2 save. This is the primary explicit save mechanism. The player learns: shelter = safety = save.
- **Biome transition save:** Crossing a biome boundary triggers a Tier 1 save + Tier 2 snapshot of the biome being left.
- **Background autosave:** Tier 1 data is silently saved every 5 minutes during gameplay. This is invisible to the player and exists only as crash protection.
- **Quit save:** Exiting the game triggers a full Tier 1 + Tier 2 save.

### Save File Format

```
save/
  profile_0/
    core.tres         # Tier 1: evolution, world flags, position
    world_state.tres  # Tier 1: biome summaries, populations
    biome_active.tres # Tier 2: current biome creature states
    biome_adj_N.tres  # Tier 2: adjacent biome states
    biome_adj_S.tres
    biome_adj_E.tres
    biome_adj_W.tres
    meta.json         # Play time, version, checksum
```

Resources (`.tres`) are used for save data because they serialize/deserialize natively in Godot with type safety. A version field in `meta.json` enables migration logic for save compatibility across game updates.

### World-State Persistence Design

The world remembers, but economically:

- **Population persistence:** Biome population counts persist. Individual creatures do not (except in active/adjacent Tier 2 saves). When a biome is reloaded from a Tier 1 summary, creatures are spawned to match the persisted population counts at statistically valid positions.
- **Event flags:** One-time world events (a bridge collapsing, a ruin being explored for the first time) are stored as boolean flags. These are cheap and permanent.
- **Ecosystem drift:** Population summaries include trend data (growing/shrinking/stable per species). Distant biomes continue to drift statistically between play sessions, so the world feels alive even where the player isn't.

---

## 5. Creature AI Architecture

### Behavioral State Machine

Each creature runs a hierarchical state machine:

```
CreatureAI
+-- Alive
|   +-- Idle
|   |   +-- Resting
|   |   +-- Grooming
|   |   +-- Sunning
|   +-- Foraging
|   |   +-- Searching
|   |   +-- Eating
|   |   +-- Drinking
|   +-- Social
|   |   +-- Mating
|   |   +-- Territorial_Display
|   |   +-- Pack_Communication
|   +-- Alert
|   |   +-- Investigating
|   |   +-- Watching
|   +-- Fleeing
|   |   +-- Running
|   |   +-- Hiding
|   +-- Hunting (predators only)
|   |   +-- Stalking
|   |   +-- Chasing
|   |   +-- Ambushing
+-- Dead
    +-- Corpse (decays, becomes food source)
```

State transitions are driven by the sensory model and weighted by species-specific behavioral data (defined in CreatureSpeciesData resources). Transition weights create species personality: a timid herbivore has high flee weights and low investigate weights. An apex predator has high hunt weights and low flee weights.

### Sensory Model

Each creature perceives the world through three sensory channels:

**Vision:**
- Defined by a sight cone (angle + range).
- Blocked by terrain and structures (raycast-based).
- Affected by lighting conditions (reduced range at night for diurnal creatures, increased for nocturnal).
- Species-specific: predators have narrow, long cones (focused hunting vision). Prey have wide, shorter cones (peripheral awareness).

**Hearing:**
- Defined by a hearing radius (omnidirectional).
- Sound events propagate outward from their source. Creatures within radius detect the event.
- Terrain partially occludes sound (reduced through walls, full through open air).
- The player's footsteps, breathing, and movement generate sound events that creatures can detect.

**Smell (simplified):**
- Wind-directional cone extending downwind from the creature.
- Detects creature presence (player or other creatures) in the downwind zone.
- Range scales with wind speed.
- Used primarily by predators for hunting and by prey for early warning.

### Food Web as Data

The ecosystem relationships are defined in a food web resource:

```gdscript
# FoodWeb.tres (Resource)
@export var relationships: Dictionary = {
    "apex_predator": {
        "prey": ["medium_herbivore", "small_predator"],
        "competitors": ["other_apex"],
        "ignores": ["insect", "small_herbivore"]  # too small to bother
    },
    "medium_herbivore": {
        "food_sources": ["grass_tile", "berry_bush"],
        "predators": ["apex_predator", "medium_predator"],
        "herd_species": true,
        "herd_size_range": Vector2i(3, 8)
    }
    # ...
}
```

The food web drives:
- Hunting target selection (predators prioritize prey species)
- Flee triggers (prey flees from predator species)
- Population equilibrium (overpopulation of prey attracts predators, overpredation crashes prey, which crashes predators)
- Spatial distribution (prey avoids predator territories, predators patrol prey-dense areas)

### Population Management

Each biome maintains population floors and ceilings per species:

| Parameter | Purpose | Example |
|-----------|---------|---------|
| `population_floor` | Minimum viable population. Below this, spawn rate increases. | 3 medium herbivores |
| `population_ceiling` | Maximum carrying capacity. Above this, spawn stops, natural attrition increases. | 12 medium herbivores |
| `spawn_rate` | Base creatures per minute when below ceiling. | 0.1 (one every 10 minutes) |
| `spawn_conditions` | Required conditions for spawning. | Time of day, weather, predator absence |
| `despawn_distance` | Distance from player at which off-screen creatures can be safely despawned. | 3 biome-widths |

Population management runs at the WorldState level, not per-creature. It operates on species counts and uses probability rather than simulating individual reproduction.

### LOD Transitions

When a biome transitions from adjacent (statistical) to active (full simulation):

1. Read population summary from WorldState.
2. For each species, spawn `count` creatures at valid positions (not in walls, appropriate terrain type).
3. Assign initial behavioral states based on time of day and weather (e.g., nocturnal creatures spawn sleeping during day).
4. Activate full AI state machines.
5. Register audio emitters with AudioManager.

When transitioning from active to adjacent:

1. Snapshot all creature states (species, count, aggregate health).
2. Store as population summary in WorldState.
3. Deactivate AI state machines.
4. Free creature nodes.
5. Deregister audio emitters.

The transition is spread across 2 seconds (approximately 120 frames) to avoid frame spikes. Creatures are spawned/despawned in batches of 4 per frame.

---

## 6. Input Architecture

### Device Support

| Device | Support Level |
|--------|--------------|
| Keyboard + Mouse | Full support, primary development target |
| Gamepad (XInput/DInput) | Full support, auto-detected |
| Steam Deck | Gamepad mode, verified layout |
| Touch | Not supported (desktop-only release) |

### InputMap Action Definitions

```
# Movement
move_left       : Keyboard A / D-Pad Left / Left Stick Left
move_right      : Keyboard D / D-Pad Right / Left Stick Right
move_up         : Keyboard W / D-Pad Up / Left Stick Up
move_down       : Keyboard S / D-Pad Down / Left Stick Down
jump            : Keyboard Space / Gamepad A (South)
crouch          : Keyboard Ctrl / Gamepad B (East)

# Actions
interact        : Keyboard E / Gamepad X (West)
ability_primary : Keyboard LMB / Gamepad RT
ability_second  : Keyboard RMB / Gamepad LT

# System
pause           : Keyboard Escape / Gamepad Start
```

All bindings are remappable through the settings menu. Remaps persist in user configuration.

### Analog Processing

- **Walk/run blend:** Left stick magnitude maps to movement speed. Below 0.4 magnitude = walk. Above 0.7 = run. Between = blend. This creates a natural analog feel and gives the player fine control over movement noise (walking is quieter than running for the creature audio system).
- **Dead zone:** Configurable per-stick, default 0.15. Applied radially, not per-axis.
- **Keyboard analog emulation:** Keyboard movement defaults to run speed. Hold Shift to walk (inverted: hold-to-walk rather than hold-to-run, because running is the more common action).

### Buffer Systems

Responsive controls require input buffering to forgive imprecise timing:

- **Jump buffer:** 100ms (6 frames). If the player presses jump within 100ms before landing, the jump executes on landing. Prevents "I pressed jump but nothing happened" frustration.
- **Coyote time:** 80ms (5 frames). After walking off a ledge, the player can still jump for 80ms. Prevents "I was on the edge" frustration.
- **Action buffer:** 50ms (3 frames). Interact and ability inputs are buffered briefly to prevent lost inputs during animation transitions.
- **Input priority:** When multiple buffered inputs would fire on the same frame, priority order is: jump > ability > interact > movement.

All buffer durations are defined in a resource file and are tunable without code changes.

---

## 7. Build and CI

### Version Control Strategy

**Trunk-based development:**
- `main` branch is always shippable.
- Feature branches are short-lived (< 3 days).
- No long-running branches. Large features use feature flags.
- Git LFS for binary assets: `.png`, `.ogg`, `.wav`, `.tres` (if binary-serialized), `.import`.

**Branching conventions:**
```
main                    # always stable
feature/creature-ai     # short-lived feature work
fix/jump-buffer         # bug fixes
spike/shader-test       # experimental, may be discarded
```

**Commit conventions:**
```
feat: add creature hearing sensory model
fix: correct coyote time frame count
perf: batch creature spawn during LOD transition
art: update forest biome parallax layers
audio: add rain surface interaction sounds
docs: update creature AI state diagram
```

### Export Configuration

| Platform | Export Template | Architecture | Notes |
|----------|----------------|-------------|-------|
| macOS | Official Godot 4.6 | Universal (x86_64 + ARM64) | Notarized, signed. App bundle. |
| Windows | Official Godot 4.6 | x86_64 | Steamworks SDK integrated. |
| Linux | Official Godot 4.6 | x86_64 | Steam Runtime compatible. |

### CI Pipeline

```
On push to any branch:
  1. Lint: gdlint (GDScript style checker)
  2. Test: GdUnit4 headless runner (all unit + integration tests)
  3. Build: Godot --headless --export-debug for target platforms

On push to main:
  4. Build: Godot --headless --export-release for all platforms
  5. Package: Create platform-specific archives
  6. Upload: Push to internal distribution (Steam partner upload for release builds)

On tag (v*):
  7. Full release pipeline with Steam depot upload
```

### Test Architecture

```
tests/
  unit/
    test_creature_state_machine.gd
    test_sensory_model.gd
    test_population_manager.gd
    test_evolution_state.gd
    test_input_buffer.gd
    test_save_serialization.gd
  integration/
    test_biome_transition.gd
    test_food_web_simulation.gd
    test_audio_mixing_states.gd
    test_shelter_save_cycle.gd
  performance/
    test_entity_budget_enforcement.gd
    test_frame_budget_compliance.gd
    test_lod_transition_smoothness.gd
```

Tests run in GdUnit4's headless mode. Performance tests assert frame timing rather than correctness. Integration tests use pre-built test scenes with known configurations.

### Asset Pipeline

```
assets/
  raw/              # Source files (Aseprite, Audacity projects) -- in LFS, not exported
  sprites/          # Exported sprite sheets (.png)
  audio/
    ambience/       # Streaming layers (.ogg)
    creatures/      # One-shot vocalizations (.wav)
    environment/    # Weather, ruin sounds (.wav)
    player/         # Player creature sounds (.wav)
  tilesets/         # Tileset images and .tres definitions
  resources/        # All .tres configuration resources
    creatures/      # CreatureSpeciesData per species
    biomes/         # BiomeData per biome
    evolution/      # EvolutionNodeData per node
    weather/        # WeatherStateData per state
```

Import settings are committed to version control (`.import` files). This ensures consistent import configuration across all contributors.
