# Diegetic UI Design Document

## 1. The No-HUD Commitment

### Why No HUD

Most games layer information on top of the world: health bars, minimaps, quest markers, ammo counters, XP indicators. This is effective, but it places a permanent screen between the player and the world. The player learns to watch the UI, not the game.

Rewild removes that screen entirely.

This is not minimalism for aesthetics. It is a direct expression of **Pillar 2: Mastery Through Understanding**. The world teaches the player. The player learns to read the world. A health bar teaches the player to watch a number. Sprite degradation teaches the player to watch their creature. A minimap teaches the player to watch a map. Environmental cues teach the player to watch the world itself.

The player who masters Rewild doesn't master a UI. They master an ecosystem.

### What Other Games Do

- **Rain World** uses minimal HUD (food pips, karma indicator) but still has UI elements. Rewild goes further.
- **Hollow Knight** uses a traditional health system (masks) and a map system. Rewild uses neither.
- **Celeste** has death counters and strawberry counts. Rewild has no counters of any kind.
- **Outer Wilds** is the closest reference -- no HUD beyond a text log, navigation by landmark and memory. Rewild adopts this philosophy for a 2D action context.

### The One Exception

The **dorsal ridge compass** is the single persistent UI-adjacent element, and it exists within the game world, on the creature's body:

- A 4x4 pixel region on the player creature's dorsal ridge (upper back/spine area).
- Subtly shifts color/brightness to indicate the direction of the nearest shelter.
- It is part of the creature's sprite -- it moves with the creature, animates with the creature, degrades with the creature.
- It is not a floating icon. It is not overlaid. It is a bioluminescent patch on the creature's body, an evolved trait that points toward safety.
- At full health: clearly visible glow. At critical health: dim, flickering. The compass degrades with the creature because it IS the creature.

---

## 2. Health Communication

### The Body Is the Health Bar

The player creature's sprite is the health display. There is no numerical health value shown anywhere. The creature's visual and audio state communicates health through four tiers:

### Tier 1: Full Health

- **Colors:** Vibrant, saturated. The creature's palette is at its richest.
- **Animation:** Smooth, full-frame animations. All movement cycles are complete and fluid.
- **Audio:** Breathing is steady and quiet. Footsteps are clean. Occasional contented vocalizations.
- **Behavior:** Full movement speed. All abilities responsive. The creature feels alive and capable.

### Tier 2: Hurt (roughly 50-75% health)

- **Colors:** Slightly muted. Saturation drops by approximately 15-20%. The change is subtle enough that the player may not consciously register it at first, but it contributes to a growing unease.
- **Animation:** Occasional hitches. The run cycle might skip a frame every few seconds. Landing from jumps has a brief recovery stutter. Nothing debilitating -- a limp that corrects itself.
- **Audio:** Breathing is slightly elevated. Occasional pain sounds on hard landings. Footsteps are unchanged.
- **Behavior:** No mechanical penalties. The visual/audio degradation is purely communicative at this tier. The player is warned without being punished.

### Tier 3: Critical (roughly 15-50% health)

- **Colors:** Significantly desaturated. The creature's palette shifts toward gray. Warm colors drain first (reds, oranges), leaving cool, washed-out tones.
- **Animation:** Visible limp in movement cycles. The creature favors one side. Jumps are labored (longer wind-up frames). Visible damage on the sprite: scratches, missing pixel clusters, darkened areas suggesting wounds.
- **Audio:** Labored breathing, audible between actions. Pain vocalizations on any significant movement. Footstep pattern is irregular (limping gait).
- **Behavior:** Movement speed reduced by 15%. Jump height reduced by 10%. The creature is mechanically compromised. The player feels the damage in the controls.

### Tier 4: Near-Death (below 15% health)

- **Colors:** Almost monochrome. The creature is nearly grayscale. Only the faintest tint of its original palette remains.
- **Animation:** Minimal. The creature can barely move. Idle animation is a shaking, labored breathing cycle. Movement is a desperate drag. No smooth transitions between states -- the creature lurches from one pose to the next.
- **Audio:** Ragged, desperate breathing. Every movement triggers pain audio. The creature's heartbeat becomes faintly audible. Footsteps are dragging, irregular.
- **Behavior:** Movement speed reduced by 40%. Jump height reduced by 30%. Abilities may be unavailable. The player's only real option is to find shelter. This tier is not meant to be played in -- it is a warning state before death.

### Health Recovery Visualization

Recovery reverses the degradation:
- Color saturation returns gradually over 3-5 seconds.
- Animation hitches smooth out.
- Audio normalizes.
- The transition from near-death to full health -- when it happens (e.g., rest in shelter after eating) -- should feel genuinely relieving. The creature comes alive again. The player sees vitality return to something they care about.

---

## 3. Navigation and Wayfinding

### The World Navigates You

There is no minimap. No quest markers. No waypoints. No compass rose in a corner. The player navigates by reading the world.

This is the most demanding design decision in the game for new players. It is also the most rewarding when mastered. The reference here is Outer Wilds: the player builds a mental map, and the satisfaction of knowing where they are by reading the environment is a core pleasure of the game.

### Environmental Navigation Cues

| Cue Type | What It Tells the Player | Example |
|----------|-------------------------|---------|
| **Light direction** | Time of day, cardinal direction (sun moves east to west) | Shadows point away from the sun. Consistent light direction helps orientation. |
| **Water flow** | Downhill direction, proximity to bodies of water | Streams always flow toward larger water bodies. Following water downstream leads to lowlands and coastal zones. |
| **Creature migration** | Seasonal/time-of-day movement patterns | Herbivores move toward feeding grounds at dawn. Following herbivores leads to vegetated areas. |
| **Vegetation density** | Proximity to water, soil quality, biome transition | Vegetation thickens near water and thins near ruins. Moss growth indicates north-facing surfaces. |
| **Ruin density** | Proximity to former urban centers | Ruin frequency increases toward the old city core. The player can navigate toward or away from human structures. |
| **Wind direction** | Consistent per-biome, carries audio cues | Wind carries sounds from adjacent areas. Hearing ocean waves means the coast is upwind. |

### The Dorsal Ridge Compass

The single concession to directed navigation:

- Points toward the nearest shelter at all times.
- The glow shifts along the creature's dorsal ridge: brighter on the side facing the shelter. The player interprets direction from the glow's position on the creature's body.
- Range: effective within 2 biome-widths of a shelter. Beyond that, the glow is dormant (very faint, no directional information). The player is truly on their own in unexplored territory.
- Intentionally imprecise. It gives direction, not distance. It says "that way" not "300 meters northeast." The player still has to navigate terrain, avoid threats, and find the actual shelter entrance.

### Mental Mapping as Core Skill

The game is designed so that biomes have strong visual identity, memorable landmarks, and consistent spatial relationships. The player builds a mental map over time:

- **Biome identity:** Each biome has a unique color palette, tile set, creature population, and ambient sound. The player always knows which biome they're in.
- **Landmark placement:** Major ruins, unique geological features, and distinctive creature territories serve as landmarks. These are hand-placed, not procedural, ensuring reliability.
- **Spatial consistency:** The world map is fixed. Biomes are always in the same relative positions. North is always up. The player's spatial knowledge accumulates and never invalidates.
- **Transition cues:** Biome boundaries are gradual, not abrupt. The player sees vegetation change, hears the ambient soundscape shift, and notices new creature species appearing over a transition zone of 2-3 screen widths.

---

## 4. Information Delivery

### No Tutorials, No Text Boxes, No Tips

The game never tells the player what to do. The game shows the player what is possible, and the player learns by watching and attempting.

### Creature Behavior as Tutorial

The most important teaching tool in Rewild is other creatures:

- **Movement tutorial:** The player sees a creature of a similar body type climb a wall, jump a gap, or swim through water. The player tries the same thing. If they can do it, they've learned an ability. If they can't, they've learned a limitation -- and possibly identified a future evolution goal.
- **Danger tutorial:** The player sees a creature get attacked by a predator. They see the predator's hunting behavior: stalking, ambushing, chasing. They learn what predators look like and how they behave before encountering one personally.
- **Feeding tutorial:** The player sees creatures eating specific food sources. They learn what's edible by observation. Eating the wrong thing has consequences. Eating the right thing has benefits. No tooltip required.
- **Shelter tutorial:** The player sees creatures retreating to sheltered areas when weather worsens or predators approach. They learn the concept of shelter from observing other creatures using it.

### Environmental Affordances

Surfaces and objects communicate their function through visual design:

- **Climbable surfaces** have visible texture variation: cracks, vines, rough stone. Smooth surfaces are not climbable. The visual language is consistent across all biomes.
- **Hazardous areas** have warning coloration: sharp contrasts, unnatural angles, visual noise. The design follows real-world aposematism (warning coloration in nature).
- **Interactive elements** have subtle animation: a lever that can be pulled has a slight wobble. A passage that can be opened has a visible seam. Nothing glows or sparkles -- but nothing is invisible either.
- **Impassable barriers** are visually distinct from traversable terrain. The player should never wonder "can I go there?" based on visual information. The answer should be apparent from the art.

### Death as Teacher

When the player dies, the cause of death remains visible:

- The predator that killed them is still in the area (or its tracks/evidence are present).
- The environmental hazard that killed them is visually apparent at the death location.
- The creature's corpse may remain briefly (depending on save tier), showing the player exactly where and how they died.
- There is no death screen with a tip. There is no "You died because..." text. The player returns, sees the world, and understands.

### Reflection Pools

Water surfaces serve as the game's character sheet:

- When the player creature stands near still water, the water shows a reflection of the creature.
- The reflection is detailed enough to show the creature's current evolution state, including any visible adaptations.
- This is the only way the player sees their creature from the front (gameplay uses a side view).
- The reflection pool is optional. The player can ignore it entirely. But for players who want to examine their creature's evolution, it's available as a diegetic alternative to a character screen.
- Mechanically: the reflection is a mirrored, slightly wavering duplicate sprite rendered on the water surface. It updates in real time as the creature changes.

---

## 5. Pause and Menus

### Pause as World Stillness

Pressing pause does not overlay a menu on the world. Instead:

1. **The world freezes.** All simulation stops. Creatures stop mid-action. Weather particles freeze. Audio fades to a low, sustained drone (the last ambient sound, held and slowly attenuated).
2. **The camera pulls back slightly** (10-15% zoom out), widening the view. This subtle shift signals "you are stepping outside the moment" without a screen transition.
3. **A minimal overlay appears** at the bottom of the screen with options: Resume, Settings, Quit. Text is rendered in the game's pixel font. No background panel -- the text sits over the frozen world.
4. **Unpausing** reverses the process: the camera zooms back in, the world resumes, audio fades back in over 0.5 seconds.

The pause state should feel like holding your breath. The world is still there. You're just not in it for a moment.

### Settings Menu

Accessible from pause. All settings are presented as simple lists with value adjustment:

**Display:**
- Window mode (Fullscreen / Windowed / Borderless)
- Resolution (integer scales of 480x270: 960x540, 1440x810, 1920x1080, 2880x1620, 3840x2160)
- VSync toggle

**Audio:**
- Master volume
- Atmosphere volume
- Creature volume
- Weather volume
- Player volume

**Controls:**
- Input device display (shows current bindings for detected device)
- Remap interface (select action, press new key/button)
- Stick dead zone slider
- Walk/run threshold slider
- Swap walk/run modifier behavior

**Accessibility:**
- Colorblind mode (Protanopia / Deuteranopia / Tritanopia adjustments)
- Audio cue toggle (enables additional audio cues for visual-only information)
- Sound caption toggle (enables text descriptions for important ambient sounds)
- Screen shake intensity slider (0-100%)
- Flash reduction toggle

### No Inventory

The creature IS the inventory. There is no item system. There are no collectibles to manage. The player's "equipment" is their evolution state -- the adaptations they've unlocked and the form they've grown into.

This means:
- No inventory screen to design.
- No item tooltips.
- No encumbrance systems.
- No crafting menus.
- The pause menu is genuinely minimal because there is nothing to manage.

The player's "progression" is visible on their creature's body. Their "loadout" is their evolved form. If they want to know what they have, they look at themselves (or find a reflection pool).

---

## 6. Accessibility Considerations

### Design Philosophy

Diegetic UI is an artistic and gameplay commitment, but it must not become an accessibility barrier. The game communicates through the world, and that communication must be perceivable by the widest possible audience.

The rule: **every piece of information communicated visually is also communicated through at least one other channel.** Every piece of information communicated through audio is also communicated visually.

### Colorblind Safety

The health degradation system relies on color saturation changes. This must remain readable for colorblind players:

- **Primary cue:** Shape and animation changes are the first-order health indicators, not color. Limping, hitching, shaking -- these are visible regardless of color perception.
- **Color adjustment modes:** Three colorblind simulation modes that remap the creature's palette to maintain visible saturation differences within the affected player's perceptible range.
- **Sprite damage markers:** At Tier 3 and Tier 4 health, visible sprite damage (scratches, missing pixels) communicates health through form, not color.
- **Biome identity:** Biomes are distinguishable by shape, creature population, and sound as well as color. No biome relies on color alone for identification.

### Audio Cue Accessibility

For players with hearing impairment:

- **Sound caption system:** When enabled, important ambient sounds (predator calls, prey alarm cascades, weather changes, shelter proximity) generate brief text descriptions at the bottom of the screen. These are styled to match the game's pixel aesthetic and fade after 3 seconds.
- **Visual predator warning:** The silence system (audio dropping before predator approach) has a visual parallel: ambient particles (dust motes, floating pollen, insects) freeze and clear from the screen as a predator approaches. This is present for all players, but it's the primary warning channel for hearing-impaired players.
- **Directional indicators:** When sound captions are enabled, sounds with directional significance (a predator to the left, water to the right) include a simple directional arrow in the caption.
- **Screen-edge flash:** When sound captions are enabled, off-screen threats cause a subtle darkening along the screen edge nearest the threat. Intensity is adjustable.

### Motor Accessibility

- **Full key remapping.** Every action can be rebound to any key or button.
- **No rapid-press inputs.** No QTEs. No button-mashing mechanics. The game is about timing and observation, not input speed.
- **Buffer window configurability.** Jump buffer and coyote time can be increased in accessibility settings (up to 200ms each) for players who need more forgiving input timing.
- **One-handed play support.** The game's input set is small enough that all actions can be bound to one hand on a keyboard or one side of a gamepad. No mandatory simultaneous two-hand inputs.

### Motion Sensitivity

- **Screen shake slider.** All screen shake effects (landing impacts, nearby explosions, thunder) are scaled by a 0-100% slider. At 0%, all shake is disabled.
- **Flash reduction.** When enabled, lightning flashes and other rapid brightness changes are smoothed to slower transitions.
- **Parallax reduction.** When enabled, parallax scroll rates are reduced by 50%, lessening the depth-of-motion effect that can cause discomfort.

### Cognitive Accessibility

- **Consistent world rules.** The game's systems are deterministic and learnable. The same action in the same context always produces the same result. The player builds reliable mental models.
- **No time pressure in menus.** Pause genuinely pauses. Settings take effect immediately. There is no urgency outside of gameplay.
- **Shelter as safe space.** Shelters are always safe. No ambushes in shelters. No timed events forcing the player to leave. When the player needs to stop and process, shelter provides that space within the game world.
