# Art Bible: Rewild

*Created: 2026-04-16*
*Status: Approved*
*Visual Direction: "Quiet Takeover"*

> **Art Director Sign-Off (AD-ART-BIBLE)**: CONCERNS (accepted) 2026-04-16

---

## 1. Visual Identity Statement

### The Cardinal Rule

**"Every pixel should look like nature solving a problem — never like an artist decorating a scene."**

Nothing in the world exists for aesthetic effect. Moss grows where moisture collects. Roots crack concrete at stress fractures. Predators are shaped by what they need to kill. If a visual element cannot answer the question *"why did this grow/erode/evolve here?"* — it does not belong in the frame.

### Supporting Visual Principles

#### 1. The Decay Gradient

**Definition:** Color palette is a stratigraphic record — the deeper you dig into a ruin or the older a surface is, the warmer and more muted it becomes; the newer the biological growth overtaking it, the cooler and more saturated it gets.

**Design test:** When the color of a surface, background layer, or environmental element is ambiguous, choose the color that tells you *how long ago nature won this particular battle* — warm ochres and concrete grays for the human past still showing through, cool blue-greens and deep moss tones for the living present growing over it. If a ruin tile and an overgrowth tile look like they belong to the same era, one of them is wrong.

**Serves Pillar 3 — Every Ruin Tells a Truth.** The player should be able to read geological time in a single screen. Human infrastructure is never gone — it is the warm, bleached skeleton beneath every living surface.

#### 2. Silhouette Ethology

**Definition:** The silhouette of every organism communicates its ecological role — apex predators read as angular and convergent (narrow, forward-facing, sharp terminal points), prey species read as dispersed and soft (wide, rounded, low-contrast outlines), and the player creature's silhouette *shifts between these poles* as it evolves.

**Design test:** When the shape language of a creature is ambiguous, choose the silhouette that a player could correctly sort into "this thing hunts," "this thing hides," or "this thing endures" *at a two-pixel scale on a zoomed-out map.* If you cannot tell what an organism does for a living from its silhouette alone, the design has failed before color is even applied.

**Serves Pillar 2 — Mastery Through Understanding.** Shape language is the first and most reliable source of ecological information — it is how the game teaches without tutorials.

#### 3. The Uninvited Frame

**Definition:** The camera, the UI, and the world composition must never center, spotlight, or aesthetically privilege the player creature — it is framed the way a nature documentary frames a small animal in a large landscape, not the way a game frames its hero.

**Design test:** When a composition choice, camera behavior, or UI emphasis is ambiguous, choose the option that makes the player feel *observed and embedded* rather than *featured and important.* If a screenshot looks like it has a main character, the framing is too generous.

**Serves Pillar 1 — The World Doesn't Care About You.** The world was not built for the player, and the camera should confirm this constantly.

### Diagnostic Sequence

For any asset, screen, or animation entering review:
1. **Decay Gradient** — Is the color telling you *when*?
2. **Silhouette Ethology** — Is the shape telling you *what it does*?
3. **The Uninvited Frame** — Is the composition telling you *you don't matter*?

If an asset passes all three, it belongs in Rewild. If it fails any one, the failure tells you exactly what to fix.

---

## 2. Mood and Atmosphere Targets

Each game state has a distinct visual identity. These are not suggestions — they are targets that every screen, palette, animation, and particle system must hit. When two states share a screen (a weather event during pursuit, observation at the edge of predator territory), the higher-intensity state dominates and the secondary state modulates it.

The overriding rule: mood is communicated through the world's behavior, never through post-processing that "tells" the player how to feel. The world gets darker because clouds move in, not because a vignette appears. The palette shifts because the biome changes, not because a color grade was applied. The camera does not emote. The ecosystem does.

---

### 2.1 Exploration (Calm)

**Primary emotion:** Solitary absorption — the specific feeling of being a small animal moving through a space that is functioning correctly without you. Not peacefulness (which implies safety is guaranteed), but the quiet attention of a creature in a zone where nothing is currently trying to kill it.

**Lighting character:** Mid-morning to early afternoon. Diffuse warm-neutral light (color temperature ~5500K equivalent in pixel terms: desaturated yellow-whites). Low contrast — shadow edges are soft, 2-3 pixel gradients rather than hard 1px lines. Light enters from the upper third of the screen at oblique angles. Dappled: broken by canopy, structural remnants, overgrowth. No surface is uniformly lit.

**Atmospheric descriptors:** Layered, breathing, overgrown, legible, unhurried.

**Energy level:** Measured. Sprite animation runs at base framerate. Parallax layers move at their slowest differential — background layers nearly static, giving a sense of depth without urgency. Ambient particle density is low: 3-5 motes per screen (pollen, drifting seeds, dust in light shafts). Nothing demands attention; everything rewards it.

**Signature visual technique — Parallax breath:** The deepest background layer shifts by 1 subpixel on a slow sine wave (~8-second cycle), creating a sense that the world itself is inhaling and exhaling. This is the only state where this effect is active. It stops the instant danger enters the frame. Its absence is the first signal that something has changed.

**Palette rules:** Full Decay Gradient is visible and readable. Warm ruin tones (ochre, concrete gray, rust) and cool biological tones (moss green, lichen teal, deep chlorophyll) coexist at their natural saturation. Neither pole is suppressed. This is the baseline palette — every other state is defined as a *departure* from this one.

**Design test:** If a calm exploration screen could be a wallpaper, it has failed. It should look like a frame from a nature documentary where nothing dramatic is happening but the camera is still rolling because the biologist knows something interesting lives here.

---

### 2.2 Tension (Danger)

**Primary emotion:** Involuntary vigilance — the specific state of a prey animal that has detected a predator's sign but not the predator itself. Not fear (that is pursuit). This is the precognitive tightening: the body knows before the mind does.

**Lighting character:** Late afternoon bleeding into dusk, or pre-dawn blue hour. Color temperature drops: warm tones in the palette desaturate by 15-20% while cool tones shift toward blue-gray. Contrast increases sharply — shadows deepen to near-black with hard 1px edges, lit surfaces narrow. Light sources become directional and singular rather than diffuse, creating strong silhouette conditions. This serves Silhouette Ethology: in tension zones, you read shape before color.

**Atmospheric descriptors:** Compressed, watchful, directional, still, silhouetted.

**Energy level:** Measured but contracted. Animation framerate remains at base, but idle animations shorten — fewer frames, tighter loops. The player creature's idle shifts from a relaxed 8-frame cycle to a 4-frame alert cycle (ears up, weight forward). Parallax differential increases slightly: foreground layers move faster relative to background, compressing perceived depth and making the space feel tighter.

**Signature visual technique — Shadow population:** Shadows in tension zones contain movement. Not literal creatures — 1-2 pixel patches within shadow areas shift position on long timers (12-20 seconds), mimicking the player's peripheral anxiety. These shadow shifts are purely atmospheric; they do not correspond to real entities. But they train the player to watch shadows, which is exactly the skill they need when a real predator is hiding in one. Particle count drops to near zero. The air goes still.

**Palette rules:** The Decay Gradient compresses. Warm ruin tones cool toward blue-gray. Cool biological tones darken. The total value range of the palette narrows — the brightest pixel on screen is 20-30% dimmer than in Exploration. The world feels like it is holding its breath. One color remains at full saturation: the player creature's most identifiable marking. This is not a concession to the Uninvited Frame — it is ecological. Prey markings are high-contrast for conspecific recognition.

**Design test:** If you can relax while looking at a tension screen, the contrast is too low and the shadows are too empty. The player should feel the urge to stop moving and listen, even though this is a still image.

---

### 2.3 Pursuit (Flight)

**Primary emotion:** Committed panic — not the anticipatory dread of tension but the kinesthetic terror of something faster than you closing the distance. The body has decided to run before the mind has finished calculating whether it should.

**Lighting character:** Lighting becomes irrelevant to mood and subordinate to legibility. Whatever the ambient light, contrast is pushed to maximum: platforms and surfaces the player can land on are the brightest elements on screen. Everything else darkens. This is not a post-process — it is the biological reality of tunnel vision. Color temperature is whatever it was, but perceived through adrenaline: warm tones burn hotter (rust becomes near-red), cool tones drain to near-monochrome blue-black. The world becomes a two-tone system: path and void.

**Atmospheric descriptors:** Tunneled, strobing, stripped, terminal, kinetic.

**Energy level:** Frenetic. Animation framerate spikes — the player creature's run cycle adds 2 frames for a sense of desperate speed. Parallax differential hits maximum: foreground elements streak past, background blurs into bands of color. Particle density surges: debris kicks up from surfaces, leaves and detritus scatter from the creature's path, disturbed insects scatter in pixel bursts. Screen shake activates — not on hits, but on near-misses: the predator's footfall, a claw strike that clips the tile behind the player. The shake communicates proximity, not damage.

**Signature visual technique — Depth collapse:** During pursuit, the parallax layers compress — background layers accelerate toward foreground speed. The world flattens. This creates a claustrophobic visual effect where escape routes feel shorter than they are and the space feels like it is closing. When the player breaks line of sight or reaches safety, the layers snap back to normal differential in a single frame. That sudden restoration of depth is the visual exhale.

**Palette rules:** The Decay Gradient is functionally suspended. The palette reduces to 3-4 functional colors: the player creature, traversable surfaces, the predator, and everything else (near-black). Warm ruin tones and cool biological tones both collapse into their darkest values. The predator's silhouette is the most readable shape on screen — angular, convergent, unmistakable. Color returns when the pursuit ends, and the sudden re-saturation is itself a reward.

**Design test:** If you can identify the biome from a pursuit screenshot, too much environmental detail survived. Pursuit should look like the same scene as tension, but with everything non-essential stripped to shadow.

---

### 2.4 Observation (Study)

**Primary emotion:** Patient fascination — the specific absorption of watching something alive do something it does not know is being watched. Not scientific detachment (too cold) and not wonder (too passive). This is the focused patience of a wildlife photographer who knows the behavior is about to happen if they do not move.

**Lighting character:** Determined entirely by the creature being observed and its habitat. If the subject is diurnal, mid-morning golden light from screen-left (the classical naturalist's light). If nocturnal, deep blue ambient with bioluminescent point sources from the subject or its environment. The key principle: the subject is lit for readability; the player creature is in shadow or partial occlusion. The Uninvited Frame is at its most literal here — the camera privileges the observed, not the observer. Contrast is moderate: enough to read behavioral detail (limb position, body language, interaction with environment) without flattening the scene.

**Atmospheric descriptors:** Intimate, granular, held, precise, documentary.

**Energy level:** Contemplative. All animation slows to its most detailed framerate — creature behaviors play at full frame count with no shortcuts. A feeding animation that runs 6 frames in normal gameplay runs its full 12-frame version here. Parallax is nearly frozen. Particle effects are hyperlocal: dust motes in the subject's immediate space, disturbed soil under its feet, breath condensation in cold biomes. The world shrinks to the subject's radius. Everything outside that radius dims by 10-15% value.

**Signature visual technique — Focus vignette (diegetic):** The screen edges do not darken through post-processing. Instead, the environmental elements at screen edges (foliage, rubble, structural remains) are rendered at reduced detail — fewer animation frames, flattened to their darkest value, functionally becoming a natural frame around the subject. This mimics peering through undergrowth. If the player moves, the "frame" shifts as different foreground elements enter and exit the edges. The effect is physical, not optical.

**Palette rules:** The Decay Gradient applies normally to the environment but the observed creature exists in its full, unmodified palette — this is the one state where a living organism displays its true colors without atmospheric suppression. This serves gameplay: the player is learning to identify this species, and accurate color is part of that identification. The player creature, conversely, is rendered in its most muted values. You are camouflage. The subject is the specimen.

**Design test:** If the player creature is more visually interesting than the observed creature, the hierarchy is inverted. The eye should forget the player exists. This screen should look like a page from a field guide, not a game screenshot.

---

### 2.5 Discovery (Ruins)

**Primary emotion:** Melancholic recognition — the unsettling feeling of finding something that was made by minds like yours, but those minds are gone and the thing they made is being slowly eaten by the world. Not horror (the ruins are not threatening). Not nostalgia (nostalgia requires memory, and the player creature has none). This is the emotion of finding a skull and understanding, without words, that it was once a face.

**Lighting character:** Interior and archaeological. Light enters from structural failures — holes in ceilings, collapsed walls, shattered windows — creating shafts that cut through darkness at steep angles. Color temperature splits: the light shafts carry warm exterior light (the living world leaking in), while the interior ambient is cool and subterranean (blue-gray to deep teal). This is the Decay Gradient made architectural: warmth comes from where nature has breached the structure; cold persists where the structure still holds. Contrast is high but localized — bright shafts surrounded by deep shadow, with midtones only where biological growth has colonized the lit areas.

**Atmospheric descriptors:** Stratified, reverential, eroded, legible, sepulchral.

**Energy level:** Contemplative, but with a different texture than Observation. Where Observation is still because you choose not to move, Discovery is still because the space demands it. Animation is slow and deliberate. Parallax layers emphasize vertical depth — stacked floors, collapsed ceilings, visible strata of human construction and biological colonization. Particles are specific to ruins: concrete dust hanging in light shafts (slow fall, long hang time), water drips catching light as single bright pixels, bioluminescent spores drifting from fungal colonies growing on old electronics.

**Signature visual technique — Chromatic archaeology:** Human-made objects retain trace amounts of their original human color palette — a color space the rest of the game never uses. Signage carries faded synthetic yellows. Plastic retains impossible oranges. Screen glass holds ghostly cyan. These colors do not occur anywhere in the biological world of Rewild. They are stratigraphic — the deeper into a ruin you go, the more of these dead colors survive, because less light and less biology has reached them. Each is desaturated and value-shifted toward the warm end, but recognizably *artificial.* When a player sees synthetic yellow, they know: a human made this. The color itself is a narrative artifact.

**Palette rules:** The Decay Gradient is inverted in vertical space. The ceiling (most exposed to biological colonization from above) carries the coolest, most saturated biological tones. The floor level (where human objects have settled and been buried) carries the warmest, most muted synthetic tones. Reading a ruin screen from top to bottom tells you the chronology of the takeover. Isolated human-spectrum colors (the synthetic yellows, oranges, cyans mentioned above) punctuate the warm base like fossils in sediment.

**Design test:** If a ruin screen looks like a dungeon from another game, the human specificity has been lost. You should be able to tell what this room was for — a classroom, a server room, a hospital ward — from the shapes and color remnants alone, even though every surface is cracked, flooded, or overgrown.

---

### 2.6 Shelter (Rest)

**Primary emotion:** Earned safety — not comfort (which implies luxury) but the specific relief of a burrowing animal that has sealed itself into a space where nothing larger can follow. The tension in the shoulders releases. The world is still dangerous, but it is on the other side of a wall.

**Lighting character:** Interior warm. A single dominant light source with a color temperature well above the game's baseline — amber to deep gold (~3000K equivalent). This is the warmest the game's palette ever gets. The light is close and contained: it does not spill beyond the shelter's boundaries. Shadows are soft and short, hugging objects rather than stretching across surfaces. No directional light from outside — the shelter is enclosed. If the shelter has an opening (a viewport to the exterior), the outside world is rendered in whatever its current lighting state is, but through the frame of the warm interior, creating a deliberate inside/outside temperature contrast.

**Atmospheric descriptors:** Enclosed, amber, denned, finite, exhaled.

**Energy level:** Contemplative at its lowest register. All animation runs at minimum framerate. The player creature's idle is its longest and most relaxed cycle — 12-16 frames of settling, adjusting, breathing. Parallax is inactive: no layers move. The world has stopped. Particles are minimal and intimate: dust motes in the amber light, occasional condensation drips. If rain or weather is occurring outside, the sound is present but the visual particles do not enter the shelter space. The boundary is absolute.

**Signature visual technique — Palette inversion:** Shelter is the only game state where the Decay Gradient reverses its emotional coding. Warm tones, which everywhere else in the game signify the dead human past, here signify present safety. This is deliberate and thematically loaded: the player creature has made a den in the bones of human civilization, and the warmth of human-era color is repurposed into biological comfort. The shelter's amber light makes ruin materials — concrete, rusted metal, old wood — look like a nest rather than a grave. The same tiles that read as melancholic in Discovery read as protective in Shelter. Context changes meaning. This is the Decay Gradient's most sophisticated application.

**Palette rules:** Compressed warm range. The palette narrows to 8-12 colors total, all within the amber-to-brown range, with a single cool accent (the exterior visible through any opening, or bioluminescent fungi providing secondary light). Saturation is low but value is high — the shelter is the brightest interior space in the game, but it is bright with warm light, not with color variety. The player creature is rendered at its warmest values, matching the shelter's palette. For once, the creature and the environment are the same temperature. It belongs here.

**Design test:** If a shelter screen looks inviting to the *player* (the human at the keyboard), it is working. This is the one state where the game is allowed to feel like it wants you to stay. But it must feel like an animal's den, not a cozy room. Comfort through enclosure, not through decoration.

---

### 2.7 Weather Events

**Primary emotion:** Systemic indifference — the world asserting that it operates on scales the player cannot influence or negotiate with. Not anger (weather has no will), not chaos (weather follows patterns). This is the specific feeling of being a 4-centimeter creature when the sky decides to do something.

**Lighting character:** Determined by weather type, but all weather events share one lighting principle: the ambient light source becomes diffuse and omnidirectional. Directional light (sun angle, shaft direction) is suppressed or eliminated.

- **Rain:** Color temperature drops to cold blue-gray (~7000K equivalent). Contrast flattens — shadows lighten, highlights dim. The value range compresses to 40% of its normal span. The world becomes a narrow band of blue-gray midtones. Visibility reduction is handled by parallax: background layers desaturate and lose detail progressively with each layer, creating a fog-of-distance effect through layer manipulation rather than an overlay.
- **Storm:** Everything rain does, but contrast inverts periodically during lightning. Lightning is a 2-3 frame full-screen flash that creates a single frame of maximum contrast (every shadow goes black, every surface goes near-white) followed by 1 frame of return. Between flashes, the baseline is darker than rain — the ambient drops by an additional 20%. The rhythm is irregular (4-12 second intervals) to prevent habituation.
- **Wind:** Color temperature is neutral but saturation drops. The visual identity of wind is kinetic, not chromatic: all foliage animations double in speed and amplitude, parallax layers shift laterally on gusts (foreground layers move opposite to wind direction by 2-4 pixels per gust), and particle density peaks — leaves, debris, seeds, pollen move horizontally across the screen in waves.
- **Fog/mist:** The rarest and most visually distinctive weather state. All parallax layers beyond the second converge to a single flat color (biome-appropriate: blue-white for highlands, green-gray for fungal zones, amber-gray for fresh ruins). The world loses depth. Foreground remains readable but everything beyond arm's reach dissolves. This is the most dangerous weather state because Silhouette Ethology becomes unreliable at distance — you cannot read shape through fog.

**Atmospheric descriptors:** Systemic, obliterating, rhythmic, layered, sovereign.

**Energy level:** Variable by type, but always elevated above the baseline state it interrupts. Rain is measured-plus. Storm is frenetic. Wind is measured but kinetically dense. Fog is contemplative but with an undercurrent of tension (reduced information). The key principle: weather does not create its own energy level — it *modulates* the energy level of whatever state it overlays. Rain during exploration feels meditative. Rain during tension feels suffocating. The same visual system, read differently by context.

**Signature visual technique — Particle stratification:** Rain and storm particles exist on multiple parallax layers, not a single overlay. Close raindrops are large (2px), fast, and sparse. Mid-distance rain is small (1px), moderate speed, dense. Far rain is sub-pixel shimmer on the background layer. This creates the illusion of volumetric rain in a 2D pixel art system. Each layer's rain speed matches its parallax speed, maintaining spatial coherence. The player creature's sprite receives a per-pixel wet shader: surface pixels darken by 10-15% value and gain a +5% saturation shift, as if absorbing water. This is subtle but accumulates — a creature that has been in rain for 30 seconds looks visibly different from one that just entered.

**Palette rules:** Weather overrides the Decay Gradient's warm/cool split. During rain and storm, everything shifts toward the cool end — even ruin tiles that are normally warm lose their warmth under overcast light. During fog, everything converges toward a single temperature determined by the biome. During wind, the palette remains normal but reads differently because nothing is still long enough to study. The cardinal rule: weather affects color by changing the light, not by recoloring the objects. A warm tile under blue-gray rain light reads as cool, but if the rain stops, it returns to warm instantly. The tile did not change. The sky did.

**Design test:** If weather looks like an Instagram filter applied over the game, it has failed. Weather is a physical event, not a mood filter. Every particle must originate from a plausible source (rain from above, debris from foliage, mist from water surfaces). Every lighting change must correspond to a sky-state change. The player should be able to tell the weather by looking at any 32x32 tile crop from the screen.

---

### 2.8 Evolution Moment

**Primary emotion:** Somatic estrangement — the uncanny sensation of your own body becoming unfamiliar. Not triumph (evolution is not a reward; it is an adaptation). Not horror (the change is not unwanted). This is the feeling of looking at your hand and seeing one more finger than you remember having. The body solved a problem you did not consciously ask it to solve.

**Lighting character:** The evolution moment inherits whatever lighting state the player is currently in — there is no dedicated "evolution lighting." However, the player creature itself becomes the dominant light source for the duration of the transformation. The creature emits a bioluminescent glow from the mutation site: the color of this glow is determined by the biome that triggered the evolution (fungal evolution glows in spore-teal, thermal evolution in deep amber, aquatic evolution in gill-blue). This glow illuminates nearby surfaces within a 2-tile radius, temporarily overriding the ambient light on those surfaces. When the transformation completes, the glow fades over 3-4 seconds and the ambient reclaims the space. The creature was briefly the brightest thing in the world. Then it wasn't.

**Atmospheric descriptors:** Involuntary, bioluminescent, liminal, irreversible, somatic.

**Energy level:** Arrested. Everything stops except the creature. All ambient animation freezes — parallax halts, particles hang in place, nearby creature sprites pause mid-frame. This is not a cutscene freeze; it is a simulation-level pause, as if the world's clock skipped a beat. The player creature's transformation animation runs at half the normal framerate (deliberately slow, 6-8 fps) to make each stage of the morphological change readable. Individual pixels on the sprite visibly shift position, change color, appear, or disappear. The player watches their silhouette change in real time. When the transformation completes, the world resumes in a single frame — particles continue their trajectories, creatures complete their interrupted actions, parallax resumes. The discontinuity is jarring. The world did not wait for you. It did not even notice.

**Signature visual technique — Silhouette delta:** During the 2-3 seconds of transformation, the creature's previous silhouette persists as a 1-frame ghost image — a faint (10-15% opacity) outline of the old form, offset by 1 pixel from the new form, lingering for exactly 1 second after the transformation completes. This ghost silhouette is rendered in the warm-muted end of the Decay Gradient palette: the old form becomes a ruin. The new form is rendered in full biological saturation. The player's own body undergoes the same warm-to-cool succession that defines the entire world. You become what you survive, and the visual language of becoming is the visual language of the world replacing what was there before.

**Palette rules:** The creature's sprite palette permanently shifts. Pixels associated with the new adaptation adopt colors from the biome that triggered the evolution — the creature literally carries the colors of its survival history in its body. A creature that evolved thermal resistance in a server-farm biome carries amber-heat tones in its adapted limbs. A creature that evolved spore membranes in fungal caverns carries teal-white on its new surfaces. Over multiple evolutions, the creature becomes a walking Decay Gradient in reverse: it is biological life that has absorbed the colors of every environment it endured. The rest of the screen's palette is unchanged. Evolution is local. The world does not celebrate it.

**Design test:** If the evolution moment feels like a "level up" screen from another game, it has failed completely. There should be no fanfare, no particle burst, no achievement frame. The creature changes. The world resumes. The player looks at their sprite and something is different, and they are not entirely sure how they feel about it. That ambivalence — *I am more adapted but I am less familiar* — is the target emotion. If the player pumps their fist, the spectacle was too rewarding. If the player feels nothing, the transformation was not visible enough. The target is a quiet "...oh."

---

### State Transition Summary

The following table maps how the visual system shifts between states. Transitions are never instant except where noted — they follow the ecological logic of the world.

| From | To | Transition Method | Duration |
| ---- | ---- | ---- | ---- |
| Exploration | Tension | Gradual palette compression, shadow deepening, parallax breath cessation | 3-5 seconds (as player enters predator territory) |
| Tension | Pursuit | Hard cut — depth collapse and palette reduction occur in 2-3 frames | Near-instant (predator commits to chase) |
| Pursuit | Exploration | Palette and parallax restore over time as predator distance increases | 5-8 seconds (deceleration curve) |
| Pursuit | Shelter | Hard transition on crossing shelter threshold — immediate warmth | Instant |
| Exploration | Observation | Diegetic focus vignette fades in as player holds still near creature | 2-3 seconds |
| Observation | Tension | Focus vignette dissolves, tension palette asserts | 1-2 seconds (observation broken by threat) |
| Any state | Weather | Parallax-layer rain/fog builds from background layers forward | 10-15 seconds (weather rolls in) |
| Weather | Clear | Reverse of onset — particles thin, color temperature restores | 15-20 seconds (weather clears gradually) |
| Any state | Evolution | World freeze, creature glow onset | Instant freeze, 2-3 second transformation |
| Evolution | Previous state | World resume, glow fade, ghost silhouette lingers | 1 second resume, 3-4 second glow fade |
| Any state | Discovery | Gradual as player enters ruin interior — chromatic archaeology emerges | 3-5 seconds (crossing threshold into ruin) |
| Discovery | Exploration | Warm/cool split reverses as player exits to exterior | 3-5 seconds |
| Exploration | Shelter | Palette inversion to warm as player crosses threshold | 1-2 seconds |
| Shelter | Exploration | Warm palette releases, full Decay Gradient reasserts | 2-3 seconds |

**Transition cardinal rule:** The world's visual state is always determined by the world's physical state. Palette shifts happen because the light changed, because the space changed, because the weather changed. If a transition cannot be explained by a physical cause, it is a UI effect and does not belong in the world layer.

---

## 3. Shape Language

Shape is the first information the player receives and the last information they forget. Before color registers, before animation plays, before context is understood, the brain has already sorted a silhouette into "safe," "dangerous," or "unknown." In Rewild, this instinct is not a design convenience -- it is the primary teaching system. Shape language replaces tutorials, threat indicators, minimap icons, and bestiary entries. If the player cannot read the ecological role of every organism and structure from its silhouette alone, the visual design has failed at its most fundamental level.

The following shape grammar governs every sprite, tile, and UI element in the game. Every rule connects back to a visual identity principle and a game pillar. No shape is decorative. Every shape is functional.

**Cardinal shape test:** Render any sprite as a solid black silhouette at 50% of its intended display size. If you cannot determine its ecological role, traversability, or origin (human-built vs. biological) from that silhouette, the shape design is wrong. Fix the silhouette before touching color.

---

### 3.1 Character Silhouette Philosophy

**Governing principle:** The player creature must be readable at any zoom level, identifiable among dozens of simultaneous organisms, and visually mutable across evolution states -- without ever becoming the most visually prominent thing on screen.

**Serves Pillar 1 (The World Doesn't Care About You) and Pillar 4 (You Become What You Survive).**

#### 3.1.1 Base Form Construction

The player creature's base form occupies a **16x16 pixel sprite canvas** at its starting evolution state. Its silhouette is built on three non-negotiable shape rules:

1. **Asymmetric mass distribution.** The body mass is concentrated in the lower 60% of the sprite. The upper 40% is lighter -- a small head, thin sensory structures (antennae, whiskers, ear-analogues). This bottom-heavy silhouette reads as "low to the ground, cautious, prey-adjacent" at any scale. It is the opposite of a hero pose. The creature looks like it is always ready to bolt.

2. **One diagnostic feature.** The silhouette contains exactly one shape element that no other creature in the game shares: a specific curve, protrusion, or appendage configuration that serves as the player's visual fingerprint. At the base evolution state, this is a **dorsal ridge** -- a 2-3 pixel line of slightly raised contour along the back, breaking the otherwise smooth upper curve. This ridge is subtle. It does not read as "special." It reads as "that specific animal." The diagnostic feature persists through all evolution states, even as the rest of the silhouette transforms around it. It is the thread of identity that lets the player recognize themselves after radical morphological change.

3. **No bilateral perfection.** The sprite is not pixel-perfectly symmetrical on its horizontal axis. One limb sits 1 pixel lower than its pair. One ear-analogue is 1 pixel shorter. This micro-asymmetry is invisible at conscious viewing distances but registers subconsciously as "organic." Human-built structures in the game are symmetrical. Biology never is. The player creature must feel grown, not designed.

#### 3.1.2 Thumbnail Readability

At the smallest legible scale (the creature rendered at 8x8 pixels on a zoomed-out view), the silhouette must resolve to five recognizable elements:

| Element | Pixel budget at 8x8 | Purpose |
| ---- | ---- | ---- |
| Body mass | 3x4 pixel block (lower center) | Establishes "organism, not terrain" |
| Head | 2x2 pixel cluster (upper, offset forward) | Indicates facing direction |
| Limbs | 2-3 single pixels extending below body | Separates creature from ground plane |
| Diagnostic ridge | 1-2 pixels on dorsal edge | "That's me, not another creature" |
| Negative space gap | At least 1px between body and any adjacent surface | Prevents the creature from merging visually with terrain |

If any of these five elements are lost, the creature has become unreadable. The sprite must be reworked to restore them.

**Design test:** Place the player creature's 8x8 silhouette on a busy screen with 6+ other organism silhouettes and 3+ overlapping terrain layers. A first-time viewer should be able to identify which silhouette is the player within 2 seconds -- not because it is brightest or most centered (the Uninvited Frame forbids that), but because the diagnostic ridge creates a unique contour that does not repeat in any other organism's shape vocabulary.

#### 3.1.3 Evolution State Silhouette Progression

The player creature's silhouette changes permanently with each evolution. These changes follow the Silhouette Ethology gradient -- as the creature adapts, its shape shifts along the prey-to-predator axis, the compact-to-dispersed axis, and the smooth-to-textured axis. The direction of shift depends on the biome that triggered the evolution.

**Five evolution silhouette principles:**

1. **Additive, not substitutive.** Evolutions add shape elements to the silhouette; they do not replace the base form. A thermal adaptation adds heat-dissipation fins (2-3 pixel protrusions on the flank). An aquatic adaptation adds a tail membrane (widening the posterior silhouette by 2-4 pixels). The base body plan remains recognizable beneath the adaptations.

2. **The diagnostic ridge persists.** No evolution may obscure, remove, or alter the dorsal ridge. Other structures may grow around it, above it, or adjacent to it, but the ridge itself remains at the same relative position and pixel count. It is the creature's signature. The player must always be able to find themselves.

3. **Silhouette complexity increases monotonically.** Each evolution makes the silhouette more complex -- more protrusions, more varied contour, more negative space interruptions. The starting creature is the simplest organism silhouette in the game (smooth, compact, few features). By late-game, it is among the most complex (textured, extended, many features). This progression is visible: a player who has evolved 6 times looks visually different from a player who has evolved twice, even if both are the same canvas size. Complexity is the visual record of survival history.

4. **Canvas expansion is earned.** The base creature occupies 16x16. After 2-3 evolutions, it may expand to 24x24. After 5-6, to 32x32. The sprite canvas never exceeds 32x32 -- the creature is never large. It is never an apex predator. Canvas expansion is gradual and biome-dependent: aquatic evolutions tend to elongate horizontally; thermal evolutions tend to add vertical height through radiator structures; fungal evolutions tend to fill negative space with textural mass.

5. **Ecological role shifts are visible in contour.** A creature that has taken primarily prey-oriented evolutions (camouflage, burrowing, speed) retains a predominantly rounded, low-profile silhouette with increased negative space (more gaps to break up the outline). A creature that has taken predator-adjacent evolutions (jaw strength, claw development, sensory enhancement) develops more angular terminal points -- sharper edges at the head and forelimbs, more convergent forward-facing geometry. The player's silhouette tells other players (in screenshot comparisons) and the player themselves what kind of survivor they have become.

**Serves Pillar 4 (You Become What You Survive).** The creature's shape is its autobiography. Every contour addition is a chapter heading.

---

### 3.2 Creature Shape Taxonomy

Silhouette Ethology (Section 1, Supporting Visual Principle 2) establishes that shape communicates ecological role. This section expands that principle into a full shape grammar -- a formal vocabulary of contour rules that determines how every organism in the game is constructed at the pixel level.

**Master rule:** The ecological role of a creature determines its shape before any other consideration. Anatomy, color, animation, and behavior are all downstream of silhouette. An artist designing a new creature begins by selecting its ecological category from the taxonomy below and constructing a silhouette that obeys that category's shape constraints. If the creature's behavior changes (a scavenger that also ambush-hunts), its silhouette must contain elements from both categories, with the primary role dominant.

**Serves Pillar 2 (Mastery Through Understanding).** This taxonomy is the grammar the player learns. Once internalized, it allows them to assess any new creature's threat level, behavior type, and ecological niche from a single glance at its silhouette -- at any distance, in any lighting condition, even during pursuit when color information is stripped.

#### 3.2.1 Apex Predators

**Ecological role:** The dominant hunters. They pursue, overpower, and kill prey through speed, strength, or both. They are the creatures the player fears most and encounters least.

**Shape constraints:**

- **Convergent silhouette.** The body mass narrows toward the front. The widest point of the silhouette is the rear or mid-body; the head and forward structures taper to sharp terminal points. This convergence reads as "directional force" -- the shape itself points forward, implying pursuit. At 16x16 or larger, the front third of the body has no more than 2 convex curves. Every forward-facing edge either terminates in a point or runs as a straight diagonal.

- **Angular terminal points.** Jaws, claws, forelimb tips, and tail ends resolve to acute angles (less than 60 degrees at the pixel level -- a single pixel projecting diagonally from a 2-pixel base). No terminal point on an apex predator is rounded. Roundness is reserved for prey. This is absolute: if an apex predator's silhouette contains a rounded terminal point, the creature reads as less dangerous, and the shape grammar has been violated.

- **Minimal negative space.** The silhouette is dense. Limbs are close to the body in resting pose. There are few gaps between body segments. The creature reads as a solid mass -- compact, efficient, nothing wasted. At 32x32, no more than 15% of the bounding box should be negative space in the resting silhouette. This density communicates armored, committed, unstoppable.

- **Horizontal dominance.** The silhouette is wider than it is tall, or at minimum reads as horizontally oriented through limb placement and body posture. Apex predators in Rewild are ground-bound or ground-proximate -- they do not float or perch. Their horizontal emphasis conveys weight and momentum. A creature that is taller than it is wide reads as a territorial defender, not a pursuer.

- **Scale exaggeration.** Apex predators are the largest creature sprites in the game (48x48 to 64x64 canvas). Their silhouette overlaps with multiple terrain tiles simultaneously. When visible, they dominate the screen not through centering or special effects, but through sheer area. This serves the Uninvited Frame: the camera does not highlight the predator, but the predator's silhouette is so large that it pulls focus through size alone.

**Pixel-level example (32x32 canvas):** The forward jaw structure is a 4-pixel diagonal line terminating in a 1-pixel point. The body mass occupies rows 8-28 (21 rows of 32) with the widest span at row 18 (24 pixels wide). The forelimbs project forward with angular tips, each limb 3 pixels wide tapering to 1. Total negative space within the bounding box: 12%. The silhouette, rendered as solid black, is unmistakably "predator" -- convergent, angular, dense, and horizontal.

**Emotional communication:** Apex predator shapes trigger the prey recognition response in the player. The convergent form, angular terminals, and horizontal mass all exploit the same visual instincts that make a real prey animal freeze when it sees a stalking cat or a cruising shark. The player should feel their posture change when an apex predator silhouette enters the screen edge.

#### 3.2.2 Ambush Predators

**Ecological role:** Hunters that kill through deception and surprise rather than pursuit. They wait, camouflage, or mimic harmless forms, then strike from concealment.

**Shape constraints:**

- **Deceptive silhouette category.** The resting silhouette of an ambush predator deliberately mimics the shape grammar of another ecological category -- typically prey or terrain. A predator that hides in fungal growth has a resting silhouette with the rounded, dispersed qualities of prey species. A predator that mimics a rock or ruin fragment has the geometric qualities of the environment category it imitates. The deception is in the shape, not just the color.

- **Hidden angular payload.** Within the deceptive silhouette, the striking apparatus (jaws, barbed limbs, constricting appendages) is folded, retracted, or occluded. When the ambush triggers, the silhouette transforms in 2-4 frames: angular terminal points emerge, the body elongates or expands, and negative space collapses as the creature unfolds. The strike silhouette shares the angular terminal rules of apex predators, but only briefly. This transformation is the visual "reveal" -- the moment the player realizes what they were looking at.

- **Silhouette instability.** The resting silhouette of an ambush predator contains one subtle wrongness -- a single element that does not quite fit the category it is mimicking. A "prey-shaped" ambush predator has one edge that is too straight, or one protrusion that is too symmetrical, or one body segment that is too dense. This wrongness is the visual puzzle the player learns to detect. It is never obvious. It is the reward for Mastery Through Understanding.

- **Scale ambiguity.** Ambush predators are sized to be mistaken for something else. Their canvas matches the canvas of whatever they mimic: a terrain-mimic occupies a 16x16 tile; a prey-mimic matches the 12x16 to 16x16 range of prey species. The player cannot use size as a reliable sorting mechanism for ambush predators. This is by design.

**Pixel-level example (16x16 canvas, resting):** The silhouette reads as a rounded, low-profile lump -- 12 pixels wide, 8 pixels tall, primarily convex curves. It resembles a resting prey animal or a mossy stone. But the left edge of the body has a 3-pixel straight segment where the folded jaw is concealed. That straight segment is the tell. When the strike triggers (16x16 expanding to 24x16 over 3 frames), the jaw unfolds: a 6-pixel angular projection with a 1-pixel terminal point extends from the left edge, and the body mass compresses vertically as the creature lunges. For 3 frames, the silhouette is pure apex-predator geometry. Then it either catches its prey or retracts to the deceptive form.

**Emotional communication:** Ambush predators weaponize the player's trust in the shape grammar. The player learns to read rounded shapes as safe -- and then an ambush predator punishes that trust. This creates a secondary learning loop: after the first ambush, the player begins scrutinizing "safe" shapes for the subtle wrongness. The game teaches through betrayal, and the ambush predator's silhouette is the instrument of that lesson.

**Serves Pillar 2 (Mastery Through Understanding).** The ambush predator is the final exam for the shape grammar. Players who have internalized the taxonomy will spot the wrongness. Players who have not will be eaten.

#### 3.2.3 Prey Species

**Ecological role:** Organisms that survive through evasion, camouflage, numbers, or environmental refuge. They are hunted by predators and are the most numerous creatures in any biome.

**Shape constraints:**

- **Dispersed silhouette.** The body mass is distributed across the sprite canvas rather than concentrated. Limbs extend at wide angles. Sensory structures (ears, antennae, eye-stalks) project outward, breaking the body's profile into a complex, broken outline. This dispersed quality makes prey harder to track against busy backgrounds -- which is the biological purpose of complex outlines in real prey animals.

- **Rounded terminal points.** Every extremity -- limb tips, tail ends, head profiles, sensory appendage tips -- resolves to a convex curve, never a point. At the pixel level, this means terminal pixels are flanked by adjacent pixels on at least two sides (creating a rounded cap), rather than projecting alone (which would create a point). This roundness is the most reliable prey identifier in the taxonomy. It is absolute: if a prey creature has a pointed terminal, it reads as a threat, and the grammar has failed.

- **High negative space ratio.** The silhouette is porous. Gaps exist between limbs and body, between sensory structures, between body segments. At 16x16, at least 40% of the bounding box is negative space. This porosity serves two functions: it makes the prey silhouette harder to resolve against textured backgrounds (ecological function: camouflage by outline disruption), and it visually communicates fragility -- the creature is not a dense mass, it is a loose assembly of parts that can scatter.

- **Vertical or neutral aspect ratio.** Prey species are as tall as they are wide, or taller. They do not have the horizontal dominance of predators. Tall silhouettes suggest alertness (an upright posture, ears or antennae raised), and a readiness to flee vertically (jumping, climbing). The vertical orientation contrasts with the predator's horizontal orientation, making the two categories distinguishable by aspect ratio alone.

- **Small canvas size.** Prey species occupy 8x8 to 16x16 canvases. They are never the largest thing on screen. Groups of prey can be present simultaneously (flocks, herds, colonies), and their individual small size allows this without visual clutter. When multiple prey are present, their dispersed silhouettes create a collective visual texture -- a field of broken, rounded shapes that the player learns to read as "safe zone, food web base."

**Pixel-level example (12x12 canvas):** The body is a 6x5 pixel rounded mass, centered vertically. Two ear-analogues project upward, each 1 pixel wide and 3 pixels tall, terminating in rounded 2-pixel caps. Four limbs extend below and to the sides, each 1 pixel wide with 2-pixel rounded feet. The gap between each limb and the body is 1 pixel. Total negative space: 52% of the bounding box. The silhouette, rendered as solid black, reads as "small, alert, fragile, not a threat" -- which is exactly the information the player needs.

**Emotional communication:** Prey shapes evoke protectiveness and safety. The dispersed, rounded quality is the visual equivalent of a bird's alarm call in reverse -- it communicates "nothing here is dangerous." The player relaxes when prey silhouettes dominate the screen. This relaxation is a calibrated tool: it makes the appearance of a predator silhouette in the same frame more startling by contrast.

#### 3.2.4 Scavengers

**Ecological role:** Opportunistic feeders that consume dead or abandoned organic matter. They are not predators (they do not hunt living prey) and not prey (they are generally unpalatable or too small to be worth hunting). They fill the ecological gaps.

**Shape constraints:**

- **Low-profile, horizontal silhouette.** Scavengers are close to the ground and wider than they are tall, but without the predator's convergent taper. The silhouette is more uniform in width from front to back -- a flattened oval or elongated rectangle with rounded corners. This low, broad shape communicates "ground-hugging, opportunistic, non-threatening."

- **Mixed terminal geometry.** Scavengers are the only category that uses both rounded and angular terminal points in the same silhouette. Their mandibles or feeding apparatus may have angular tips (they need to break through tough material), but their limbs and body contour are rounded. This mixed grammar is the scavenger's visual fingerprint -- it reads as "not dangerous to you, but not harmless either." The angular elements are always concentrated at the head or mouth area and never exceed 30% of the total terminal count.

- **Moderate negative space.** Between the dense apex predator (15%) and the porous prey (40%+), scavengers sit at 25-30% negative space. They are sturdier than prey but less imposing than predators. Their silhouettes have clear body segments (head, thorax, abdomen in insectoid forms; head, body, tail in vertebrate forms) separated by visible but narrow gaps.

- **Surface-conforming posture.** Scavenger silhouettes hug the terrain line. Their lowest pixels are nearly flush with the surface they occupy, with minimal limb extension below. This creates the impression that the scavenger is part of the surface layer -- present but unimportant, part of the environmental texture rather than an actor in the ecological drama.

- **Small to medium canvas.** Scavengers occupy 8x8 to 16x16 canvases. They can be present in large numbers without visual clutter because their low profiles merge with the terrain texture at distance.

**Pixel-level example (16x8 canvas):** The body is a 12x5 pixel horizontal mass, flattened. The head projects forward with two mandible-analogues: each is a 2-pixel line terminating in a 1-pixel angular tip. The rear is rounded (3-pixel convex curve). Six legs extend below, each 1 pixel, flush with the ground plane. Negative space: 28%. The silhouette reads as "beetle-like, ground-level, busy, not a threat" -- part of the scene's texture, not its drama.

**Emotional communication:** Scavenger shapes communicate normalcy and industry. Their presence tells the player that the ecosystem is functioning -- things are dying, things are being consumed, the cycle continues. A screen full of scavengers reads as "recently dangerous, currently safe" -- something died here, and the cleanup crew has arrived. Their visual mundanity is reassuring.

#### 3.2.5 Territorial Defenders

**Ecological role:** Organisms that do not hunt beyond their territory but aggressively defend a fixed area. They are dangerous only when their boundary is crossed. They are obstacles, not pursuers.

**Shape constraints:**

- **Vertical dominance.** Territorial defenders are taller than they are wide. This is the primary shape differentiator from apex predators (horizontal dominance) and prey (neutral aspect). The vertical silhouette communicates "planted, immovable, blocking." A territorial defender's height is its threat -- it occupies vertical screen space the same way a wall does.

- **Symmetrical silhouette.** Unlike every other biological category (which uses micro-asymmetry), territorial defenders have notably symmetrical silhouettes. Their left and right halves mirror each other within 1-pixel tolerance. This symmetry reads as architectural -- the defender is more like a structure than an animal. It is rooted, stable, and predictable. The symmetry also serves gameplay: the player can read the defender's threat zone as symmetrical around its body, which it is.

- **Blunt terminal points.** Terminal points are neither rounded (prey) nor acute (predator). They are squared or truncated -- 2-3 pixel flat edges at the ends of limbs, horns, or body projections. This blunt geometry communicates "not designed to chase, designed to withstand." Blunt terminals are the shape language of armor, of walls, of the thing that does not need to be sharp because it does not need to pursue. It only needs to hold.

- **Minimal negative space.** Like apex predators, territorial defenders have dense silhouettes (15-20% negative space). But where the predator's density reads as "efficient killing machine," the defender's density reads as "mass, weight, immovability." The difference is in the aspect ratio: horizontal density pursues, vertical density blocks.

- **Medium to large canvas.** Territorial defenders occupy 24x32 to 32x48 canvases. They are larger than prey and scavengers but often smaller than apex predators. Their visual impact comes from height rather than area.

**Pixel-level example (24x32 canvas):** The body is a 16x20 pixel mass, vertically oriented. Two horn-analogues project upward from the head: each is 3 pixels wide with a flat 2-pixel terminal edge. Four legs plant into the ground, each 3 pixels wide with flat 3-pixel bases. The left and right halves are mirrored. Negative space: 18%. The silhouette reads as "wide, tall, planted, blocking passage" -- which is functionally what the creature does. The player sees this shape and understands: do not go through, go around.

**Emotional communication:** Territorial defender shapes evoke the feeling of encountering a locked gate. The vertical, symmetrical, blunt-edged silhouette communicates boundary and consequence without implying pursuit. The player feels "I can see where I cannot go" rather than "something is chasing me." This is a fundamentally different kind of threat, and the shape language makes that difference legible at a glance.

**Serves Pillar 1 (The World Doesn't Care About You).** Territorial defenders are not guarding anything for narrative reasons. They defend their territory because that is what their species does. The shape communicates the mechanical reality: this space belongs to something that will hurt you if you enter it.

#### 3.2.6 The Player Creature: Shape Migration

The player creature is unique in the taxonomy because its silhouette is not fixed to a single category. It begins as a prey-adjacent form and, through evolution, migrates across the shape grammar toward whatever ecological role its adaptations support. This migration is the visual expression of Pillar 4 (You Become What You Survive).

**Shape migration rules:**

| Evolution type | Shape shift direction | Specific silhouette changes |
| ---- | ---- | ---- |
| Speed/evasion adaptations | Deeper into prey grammar | More negative space, thinner limbs, taller sensory structures, more pronounced rounded terminals |
| Sensory enhancements | Prey grammar + unique extensions | Larger sensory appendages (antennae, lateral line analogues) projecting from the base form, increasing silhouette complexity without changing the body's prey-grammar core |
| Jaw/claw development | Toward predator grammar | Angular terminals appear on forelimbs and head. Forward convergence increases. The creature's front third sharpens while the rear retains prey-grammar roundness, creating a hybrid silhouette |
| Armor/shell growth | Toward defender grammar | Body density increases (negative space decreases). Silhouette becomes more symmetrical. Blunt terminal edges appear on new carapace structures. The creature reads as "sturdier, slower, more planted" |
| Burrowing adaptations | Toward scavenger grammar | The silhouette flattens horizontally. Profile height decreases. The creature becomes more ground-conforming, with wider, paddle-like limbs |
| Thermal/chemical resistance | Category-neutral additions | Radiator fins, venting structures, or membrane growths add silhouette complexity without shifting the grammar toward any specific category. These read as "specialized" rather than "reclassified" |

**The hybrid silhouette problem:** A late-game creature that has evolved across multiple categories will carry shape elements from several grammar sets. A creature with jaw development AND armor growth has angular forward terminals AND dense symmetrical mass -- it looks like a predator-defender hybrid. This is intentional. The player creature's silhouette is meant to become increasingly hard to categorize as the game progresses. It should not fit neatly into any box. It should look like something the ecosystem has not seen before -- because it is.

**Serves Pillar 4 (You Become What You Survive).** The shape migration system means the player's creature is a visual argument for its own survival history. Another player looking at a screenshot of a late-game creature can read, from the silhouette alone, what that creature has been through: "angular jaw, dense torso, thermal fins -- this player fought, armored up, and survived the server farms."

---

### 3.3 Environment Geometry

The environment's shape language carries the same communicative burden as the creature taxonomy. Where creature shapes tell the player "what does this do?", environment shapes tell the player "what happened here?" and "when did it happen?" The shape grammar of the environment is the spatial expression of the Decay Gradient -- it tells time through geometry.

**Serves Pillar 3 (Every Ruin Tells a Truth).** The shape of a surface tells you whether a human built it, whether nature replaced it, or whether the two are currently fighting for it.

#### 3.3.1 Human-Built Structures (Pre-Decay)

**Shape identity:** Euclidean geometry. Right angles, parallel lines, uniform spacing, bilateral symmetry, gridded repetition.

Human architecture in Rewild follows the shape language of real human construction: it is rational, regular, and repetitive. This is not a stylistic choice -- it is a narrative one. Human civilization built in straight lines because it could. The regularity of human geometry is itself a statement about the cognitive capability that is now gone.

**Shape constraints for human-built tiles and structures:**

- **90-degree corners.** Every intact human-built corner is a true right angle. At the pixel level, this means a 1-pixel step change in direction with no diagonal pixels softening the transition. Where Godot's pixel grid makes true 90-degree corners trivial, the art direction leverages this: human corners are the sharpest, cleanest shapes in the game. They are conspicuous because nothing biological shares this quality.

- **Parallel repetition.** Human structural elements repeat at regular intervals: window columns, floor tiles, rebar spacing, cable conduit runs, ventilation ducts, ceiling panel grids. The interval is consistent within a structure: if windows are spaced 8 pixels apart, every window in that structure is spaced 8 pixels apart. This regularity is the visual signature of design intent -- someone planned this. The regularity is also immediately visible when disrupted: a missing panel, a collapsed section, a biological intrusion breaking the grid.

- **Bilateral symmetry.** Human structures are symmetrical around their central vertical axis. Doorways are centered. Facades are mirrored. Staircases have equal runs on both sides. This symmetry is never broken by design -- only by decay. Where the symmetry is intact, the structure is intact. Where it is broken, something happened.

- **Flat horizontal planes.** Floors, ceilings, shelves, countertops, and ledges are horizontal. They are level. This is the most alien shape quality in the post-human world, because nothing biological grows level. A flat horizontal surface is an immediate visual signal: "a human made this." In gameplay terms, flat horizontal surfaces are also the most reliable platforms, which creates a feedback loop: human ruins are the most traversable spaces because human geometry is the most predictable geometry.

- **Uniform stroke width.** The outlines of human structural elements maintain a consistent pixel width. A 2-pixel-wide wall remains 2 pixels wide along its entire length. A rebar line is 1 pixel everywhere. This uniformity is manufactured precision. It reads as "machine-made, intentional, calibrated."

**Tile construction rule:** Human-built tiles are assembled on an 8x8 sub-grid. Structural lines align to this grid. This makes human architecture feel modular and systematic, which it is. The grid alignment also means human tiles tessellate cleanly -- they were designed to fit together. This tessellation is part of the visual story: humans built in systems, and the systems are still visible even as they crumble.

#### 3.3.2 Biological Growth (Post-Human)

**Shape identity:** Fractal irregularity. Branching structures, curved surfaces, variable thickness, radial asymmetry, non-repeating patterns.

Biology in Rewild follows the shape language of real organic growth: it is responsive, irregular, and opportunistic. No two biological elements are shaped the same way because no two biological elements grew in the same conditions.

**Shape constraints for biological tiles and structures:**

- **No right angles.** Biological shapes never produce a true 90-degree corner. At the pixel level, every direction change in a biological outline includes at least 1 diagonal pixel softening the transition. This is the most reliable discriminator between human and biological geometry. A player who has internalized this rule can instantly distinguish a biological edge from a human edge, even at 1x zoom, even in deep shadow, even during pursuit when color information is stripped. The absence of right angles is the shape signature of life.

- **Branching hierarchy.** Biological structures follow branching rules: thick at the base, thinner at each division, thinnest at the terminal. Roots, vines, fungal hyphae, lichen formations, and moss colonies all obey this hierarchy. At the pixel level: a primary vine is 3 pixels wide at its base, branches to 2 pixels, and terminates at 1 pixel. This branching is fractal -- it repeats at every scale, from a 64-pixel root system to an 8-pixel moss colony. The player learns to read branching depth as a proxy for age: deeper branching means older growth, which means this surface was reclaimed long ago.

- **Variable stroke width.** Unlike the uniform strokes of human construction, biological outlines vary in width along their length. A vine is 3 pixels wide where it grips a wall, 2 pixels where it spans open air, and 1 pixel at its growing tip. This variability communicates responsiveness -- the organism grew thicker where it needed to grip, thinner where it needed to reach.

- **Non-repeating pattern.** No two biological tiles are identical. Where human tiles tessellate through grid repetition, biological tiles create continuity through shared shape language (branching, curvature, variable width) without shared exact geometry. In practice, this means creating 3-4 visual variants for each biological tile type and distributing them with weighted randomness. The player should never see a repeating wallpaper pattern in biological growth.

- **Surface conformity.** Biological shapes wrap around, grow through, and conform to whatever surface they colonize. A vine on a wall follows the wall's surface. Moss fills the concavities of a concrete crack. Lichen spreads along the moisture gradient of a ceiling joint. Biological shapes are reactive to the geometry beneath them. They never float free of their substrate.

**Tile construction rule:** Biological tiles are NOT aligned to the 8x8 sub-grid. Their structures overshoot tile boundaries, bleed between adjacent tiles, and create visual continuity that ignores the underlying grid. This ungridded quality is itself communicative: biology does not respect the systems that humans built. It grows where conditions allow, not where a grid says it should.

#### 3.3.3 The Collision Zone

**Shape identity:** Geometric disruption. Human regularity being deformed, penetrated, fractured, and consumed by biological irregularity.

The collision zone is where the Decay Gradient becomes spatial. It is the most visually rich and narratively dense environment in the game, and its shape language must communicate the ongoing process of biological takeover in every tile.

**Shape constraints for collision zone tiles:**

- **Interrupted parallels.** Human parallel lines (walls, floors, conduit runs) continue but are interrupted by biological intrusions. A wall that runs for 24 pixels is broken by a 4-pixel vine penetrating through it at pixel 14, creating a gap with organic edges where the concrete has been displaced. The parallel resumes on the other side, offset by 1-2 pixels (the wall has shifted). This interruption pattern tells the player: "this was straight, something broke it, it is still recognizably straight on either side of the break."

- **Softened corners.** Human right angles are being rounded by biological action. A concrete corner that was once 90 degrees now has 2-3 pixels of diagonal transition where moss, erosion, or root pressure has worn the edge. The degree of softening indicates the stage of decay: 1 pixel of softening is early decay; the full right angle is still readable. 4+ pixels of softening means the corner is nearly gone; the human origin is barely legible.

- **Grid distortion.** The 8x8 sub-grid of human construction is still present but warped. Floor tiles that were once perfectly aligned now have 1-2 pixel shifts between them where roots have pushed from below. Window columns that were evenly spaced now have variable gaps where structural members have buckled. The grid is recognizable but damaged -- like reading a book that has been left in the rain. The words are still there, distorted.

- **Hybrid outlines.** Individual tiles in the collision zone contain both uniform-width human outlines and variable-width biological outlines, sometimes in the same structural element. A rebar line (1 pixel, uniform) emerges from concrete, bends where a root has pushed it, and is wrapped by a vine (variable width, branching). The transition point -- where the straight becomes curved, where the uniform becomes variable -- is the visual epicenter of the collision. These transitions should be frequent and legible. Each one tells a micro-story of structural failure and biological opportunity.

- **Negative space as narrative.** In the collision zone, gaps and voids have meaning. A hole in a ceiling means water comes in. A gap in a floor means roots have broken through from below. A void behind a wall means biological mass has displaced the fill material. Every negative space in a collision zone tile should be answerable: "What grew here? What fell? What was displaced?" If a gap cannot answer that question, it is decorative, and the Cardinal Rule prohibits it.

**Tile construction rule:** Collision zone tiles begin as human-grid tiles and are manually disrupted by the artist. The artist places the biological elements second, always in response to the human geometry: "A vine grows here because this wall crack admits light. Root pressure displaces this floor tile because there is soil 2 meters below. Moss colonizes this ceiling corner because moisture condenses at the junction." The artist is simulating ecological process, not decorating a surface.

**Serves Pillar 3 (Every Ruin Tells a Truth) and the Cardinal Rule.** Every pixel in the collision zone must answer: "Why did this grow/erode/evolve here?" The collision zone is where the Cardinal Rule is tested most rigorously and where violations are most visible.

#### 3.3.4 Biome Shape Differentiation

Each biome in Rewild has a dominant biological shape vocabulary that distinguishes it from every other biome. This differentiation must be legible from shape alone (before color), because the Decay Gradient's color shifts between biomes are often subtle in adjacent zones.

| Biome type | Dominant biological shape | Human structure state | Shape signature (what makes it unique at a glance) |
| ---- | ---- | ---- | ---- |
| **Fresh ruin** (early decay) | Sparse, thin-stemmed pioneers: grasses, mosses, small ferns. 1-pixel stems, 2-pixel leaf clusters. Low coverage -- human geometry dominates 70%+ of the tile area | Mostly intact. Right angles present. Grid readable. Surfaces stained but unbroken | The ratio of straight to curved edges is the highest in the game. The environment is still mostly Euclidean |
| **Mid-decay** (collision dominant) | Aggressive structural colonizers: thick vines, root networks, woody shrubs. 2-3 pixel stems with branching. Coverage approaches 50-50 | Structurally compromised. Walls cracked, floors buckled, ceilings partially collapsed. Right angles softened but recognizable | The highest density of hybrid outlines (human + biological in the same structural element). Visually the busiest biome |
| **Deep reclamation** (biological dominant) | Canopy and understory formation: trees growing through structures, fungal colonies covering walls, dense ground cover. 3-4 pixel trunks, full branching hierarchies | Skeletal. Only the heaviest structural members (foundations, load-bearing walls, rebar grids) survive. Most surfaces are gone | The human grid is visible as a faint skeleton beneath overwhelming biological mass. Straight lines are rare and always partially occluded |
| **Fungal cavern** | Radial growth patterns: circular colony edges, spherical fruiting bodies, branching hyphae networks. The dominant shape primitive is the circle and the radial spoke | Subterranean human structures (tunnels, basements, server rooms) with surfaces fully colonized. Geometry readable only in floor planes and structural beams | Circles. This is the only biome where circular shapes dominate. Fruiting bodies are 4-8 pixel circles. Colony edges are arcs. The roundness distinguishes fungal caverns from every other biome instantly |
| **Thermal zone** (server farms, industrial) | Heat-adapted spiky growth: crystalline mineral formations, heat-resistant succulents with thick angular leaves. The dominant shape is the angular protrusion -- but biological, not human | Distorted by heat. Metals warped, concrete spalled, surfaces bubbled. Human geometry is recognizable but plastically deformed, as if the straight lines melted slightly | The only biome where biological growth is angular. This inverts the normal shape grammar: here, the biology is spiky and the ruined human geometry is softened. The inversion is jarring and signals danger |
| **Aquatic/flooded** | Flowing, draping shapes: algae curtains, submerged root networks, floating plant mats. Shapes are horizontally elongated and have trailing edges that suggest current direction | Submerged to varying depths. Water line visible on walls. Below the water line: softened and occluded by aquatic growth. Above: standard decay for the zone | Horizontal trailing edges. Every biological shape in the aquatic biome has a directional quality -- shapes taper downstream. This directional consistency is unique to this biome and communicates water flow without a particle effect |
| **Canopy/treetop** | Vertical climbing forms: epiphytes, aerial roots, vine networks climbing into light. Shapes are vertically oriented with upward-tapering terminals. Light gaps create strong negative space patterns | Upper stories of tall structures (skyscrapers, bridges, overpasses, transmission towers). Only the structural framework remains -- floors and walls gone, leaving geometric skeletons | The highest negative space ratio of any biome. The environment is more gap than surface. The remaining human framework is the thinnest (1-pixel structural lines) and most geometric, creating a stark contrast with the climbing biological forms |

**Biome transition rule:** Biome boundaries are never hard lines. Over a horizontal span of 3-5 screens, the dominant biological shape vocabulary of one biome gradually gives way to the next. The transition is accomplished by decreasing the frequency of one biome's shape set and increasing the other. In the middle of the transition, both vocabularies coexist at roughly equal frequency, creating a visual ecotone -- a shape-level indication that the player is crossing from one ecological zone to another.

---

### 3.4 UI Shape Grammar

**Governing principle:** The UI does not exist.

This is an overstatement for precision. UI elements are present in Rewild, but they obey one absolute rule: no UI element may use a shape, a line quality, or a spatial logic that does not already exist in the world. The UI is made from the world's shape language, not from a separate "interface" shape language. There is no HUD chrome, no floating panels, no overlay frames, no button bevels. The UI is diegetic or invisible.

**Serves Pillar 1 (The World Doesn't Care About You) and the Uninvited Frame.** A visible, designed UI implies a system that is tracking the player, measuring their progress, and presenting information for their convenience. In Rewild, no such system exists within the fiction. The camera is not the player's ally. The interface is not the player's dashboard. Any information the player receives must be available through the same channels available to any creature in the world: sight, sound, and body awareness.

#### 3.4.1 What the Player Knows (and How Shapes Communicate It)

| Information type | World-layer communication method | Shape language used |
| ---- | ---- | ---- |
| **Health/vitality** | The player creature's sprite itself. Damage is communicated through silhouette degradation: pixels at the extremities of the sprite flicker, dim, or temporarily disappear. A badly damaged creature has a visibly incomplete silhouette -- ragged edges, missing limb pixels, a thinner profile. The creature's idle animation becomes shorter and more constrained. The player reads their health from their own body, the way an animal does | Prey grammar (rounded forms becoming more dispersed and fragmented as health drops). The creature looks like it is falling apart, which it is |
| **Evolution readiness** | Biome-specific visual feedback on the creature sprite. When the player has survived long enough in a biome to trigger evolution potential, the relevant body region begins a subtle 2-frame bioluminescent pulse (1 pixel brightening on a 4-second cycle). This is a body signal, not a UI signal -- the creature's cells are preparing to change | No new shapes introduced. The existing silhouette pulses. The shape remains constant; only value shifts |
| **Threat proximity** | The creature's behavioral animation shifts from relaxed idle (8-frame, slow) to alert idle (4-frame, ears up, weight forward) when a predator enters detection range. This is not a UI indicator -- it is the creature's own instinct, communicated through the same body language that real prey animals use | The creature's silhouette subtly shifts from prey-at-rest (lower profile, relaxed limbs) to prey-alert (taller profile, limbs gathered, sensory structures extended). A 2-3 pixel height increase in the silhouette |
| **Biome information** | The environment itself. The shape grammar of the surrounding tiles tells the player everything: what biome they are in (shape vocabulary), how decayed the human structures are (collision zone density), what ecological pressures exist (organism silhouettes present). No map overlay, no biome label, no compass | Environmental shape grammar as defined in Section 3.3 |
| **Navigation** | Traversable surfaces are distinguished from non-traversable surfaces by shape alone. Platforms the player can stand on have flat-top contours (at least 3 consecutive horizontal pixels at the same height). Non-traversable surfaces have no flat runs -- they are uniformly curved, sloped, or broken. The player learns to read "flat top = safe footing" the way a real animal reads terrain | Human geometry (flat horizontal planes) vs. biological geometry (curved, irregular surfaces). The most reliable platforms are the most human-shaped surfaces, which creates a dependency on ruin infrastructure that is thematically resonant |
| **Pause/menu** | When the player pauses, the world does not freeze -- it dims. The screen reduces to 20% brightness, and the creature enters a curled rest posture (a compact, rounded silhouette occupying 60% of its normal canvas). Menu options, if any, are rendered as etched marks on the nearest surface -- scratched into concrete, scored into bark -- using the shape language of the surface material. They are not floating UI elements. They are marks on a wall | Menu text and icons use the shape grammar of the environment material they are displayed on. On concrete: straight, scratched, Euclidean. On biological surfaces: curved, organic, following the grain of the material |

#### 3.4.2 What the UI Does Not Do

- No health bar. No stamina bar. No resource counter. No experience indicator. No minimap. No quest tracker. No damage numbers. No waypoints. No button prompts. No tooltip frames.
- No UI element uses a stroke width, corner radius, or spatial arrangement that does not exist in the world's shape language.
- No UI element appears in a fixed screen position. If information is communicated, it is communicated from the world (the creature's body, the environment's surfaces, the behavior of nearby organisms), not from a persistent overlay anchored to the viewport.

**The single exception:** A very small, semi-transparent directional indicator appears at the screen edge when the player's shelter (last rest point) is off-screen. This indicator uses the shape of the creature's diagnostic ridge (the dorsal contour unique to the player) rendered at 4x4 pixels, pointing toward the shelter. It fades to full transparency when the player is within 3 screens of the shelter. It uses no color not already present in the creature's palette. It is the one concession to navigational convenience, justified diegetically as the creature's homing instinct.

---

### 3.5 Hero Shapes vs. Supporting Shapes: Readability Without Privilege

**The problem this section solves:** In conventional game design, the player character is the most visually prominent element on screen -- the brightest, the most detailed, the most centered, the most animated. Rewild's Uninvited Frame principle forbids all of these tools. The player creature is not brighter, not more detailed, not centered, and not more animated than its surroundings. Yet the player must always be able to find their creature on screen within 1-2 seconds, even in visually dense environments with dozens of organisms and layered terrain.

**The solution is shape uniqueness, not shape prominence.**

The player creature is not louder than its environment. It is a different word in the same language. It is readable not because it shouts, but because it says something no other element says.

#### 3.5.1 The Diagnostic Ridge Principle (Extended)

The diagnostic ridge described in Section 3.1 is the keystone of readability without privilege. It works because it occupies a shape space that is deliberately kept empty in the rest of the shape grammar:

- No prey species has a dorsal ridge with the player's specific curvature.
- No predator species has a feature at the dorsal position.
- No scavenger species has a protruding contour in the same pixel range.
- No terrain tile produces a contour that mimics the ridge at any rotation.

This is not achieved by making the ridge visually loud. It is achieved by making the ridge visually *unique* -- a shape that occurs exactly once in the entire game's visual vocabulary. The player finds their creature not by scanning for brightness, but by scanning for the one contour that occurs nowhere else. This is how real animals find conspecifics in a crowded environment: not by being the most visible, but by having a species-specific visual signature.

**Implementation rule:** When designing any new creature, terrain tile, or environmental structure, the artist must verify that the new element's silhouette does not produce a contour within 2 pixels of the player's diagnostic ridge shape at any rotation or scale. If it does, one of the two designs must be modified. The diagnostic ridge's shape space is reserved.

#### 3.5.2 Depth-Layer Shape Separation

Rewild uses 4-5 parallax layers. Each layer has a distinct shape treatment that helps the player's eye sort foreground action from background texture:

| Layer | Shape treatment | Purpose |
| ---- | ---- | ---- |
| **Background (deepest)** | Broad, low-detail shapes. Contours use 3-4 pixel minimum stroke. No single-pixel features. Silhouettes are simplified to their most basic geometric primitives | Reads as "distant, not interactive." The visual simplification prevents the player from trying to parse background shapes as organisms or platforms |
| **Mid-background** | Moderate detail. 2-pixel minimum stroke for structural elements. Some single-pixel features on biological growth | Reads as "near but not here." Creates depth without competing for attention with the gameplay layer |
| **Gameplay layer (ground plane)** | Full detail. 1-pixel features permitted. All creature and platform shapes at their maximum complexity and sharpness | This is where the player creature, other organisms, and interactive surfaces exist. All shape grammar rules in this document apply at full resolution to this layer |
| **Near foreground** | Full detail but partial occlusion. Elements on this layer overlap the gameplay layer at the screen edges, creating the diegetic focus vignette described in Section 2.4. Foreground shapes are biological (foliage, vines, hanging roots) with the organic shape grammar at full complexity | Creates the sense of peering through undergrowth. Partially occludes the screen edges without obscuring the central gameplay area. In the Observation game state, this layer becomes the primary framing device |
| **Particle/weather layer** | Minimal shape. 1-2 pixel dots, single-pixel lines (rain), small irregular clusters (debris). No recognizable silhouettes | Weather and atmospheric particles are shape-neutral -- they do not trigger the organism-recognition or structure-recognition circuits. They are visual texture, not visual information |

**The gameplay layer is where readability matters.** The player's eye learns to focus on this layer because it is the only layer where full-resolution shape grammar operates. The diagnostic ridge only exists at gameplay-layer resolution. Creatures only exist on the gameplay layer. The separation of shape complexity by depth creates an automatic visual hierarchy that does not require spotlighting or centering the player.

#### 3.5.3 Motion Contrast Over Shape Contrast

In a game where the player creature's shape, size, color, and screen position are not privileged, the most reliable readability tool is motion differential. The player creature moves in response to input; everything else moves in response to AI, physics, or animation cycles. This responsiveness is the ultimate visual identifier.

**Shape-motion coupling rules:**

- **Player creature motion is input-synchronous.** The creature's silhouette changes shape in immediate response to input -- the instant the player presses a direction key, the silhouette shifts to the movement pose. No lag, no anticipation frames. This synchrony is unique. Every other creature has animation lead-in frames (1-3 frames of anticipation before movement begins). The player's creature is the only organism whose silhouette changes in sync with an external will. The player does not consciously notice this, but their pattern-recognition systems identify "the thing that moves when I move" as their avatar.

- **Player creature silhouette state changes are more frequent.** Because the player is constantly providing input (moving, stopping, jumping, crouching, observing), the player creature's silhouette changes state more often than any AI-driven creature. AI creatures hold poses for longer intervals (2-5 seconds in idle, 1-3 seconds between action states). The player creature transitions between silhouette states every 0.3-1 second during active play. This frequency makes the player creature the most "active" silhouette on screen -- not the brightest, not the biggest, but the most kinetically variable.

- **Input cessation reveals identity.** When the player stops providing input, the creature's transition to idle is immediate (1-2 frames). AI creatures transition to idle gradually (4-8 frames of deceleration). If a player loses track of their creature and stops all input, the creature that instantly goes still is theirs. This is a shape-grammar-level identification tool: the transition curve of the silhouette from motion to rest is different for the player creature than for any other organism.

#### 3.5.4 What Recedes

For hero-shape readability to work through uniqueness rather than prominence, supporting shapes must actively recede. Recession is not dimming or blurring -- it is shape-grammar compliance that makes elements predictable, and therefore parseable at lower cognitive cost.

**Shapes that recede (and how):**

- **Repeated organisms.** A flock of 8 identical prey creatures recedes because the brain groups them into a single perceptual object. Their identical silhouettes become texture, not individual elements. This is a Gestalt grouping effect. The player creature's unique diagnostic ridge prevents it from being grouped with any flock, so it pops out of the texture.

- **Static terrain.** Tiles that do not animate are parsed once and then deprioritized by the player's visual system. The brain stops allocating attention to shapes that do not change. The player creature, which changes silhouette constantly (see motion contrast above), cannot be deprioritized. It demands ongoing attention because it is ongoing information.

- **Gridded structures.** Human-built structures with regular repeating geometry (window grids, floor tile patterns, rebar arrays) recede because their regularity makes them predictable. The brain anticipates the next element in a regular pattern and stops actively processing it. Irregular shapes (biological growth, the player creature, predators) break the pattern and attract attention.

- **Background-layer shapes.** The simplified, broad-stroke shapes on deeper parallax layers are designed to be parseable in peripheral vision. They communicate "sky," "distant structure," "far canopy" without requiring foveal attention. The player never needs to focus on them, so they never compete with gameplay-layer readability.

**What draws the eye (involuntarily):**

1. Any silhouette that is new -- not yet encountered in this biome. Novelty is the strongest shape-level attention cue.
2. Any silhouette that is large -- apex predator scale (48x48+). Size is a primal attention driver.
3. Any silhouette that moves against the prevailing motion direction -- a creature moving left when parallax and wind move right.
4. Any silhouette that changes category -- an ambush predator unfolding from prey grammar to predator grammar. Shape-category violations trigger alert.
5. The player's own creature -- found via diagnostic ridge uniqueness and motion synchrony, not via brightness or centering.

**This hierarchy is deliberate.** A novel predator entering the frame should draw attention before the player creature does. The player should see the threat before they see themselves. This is not a readability failure; it is Pillar 1 in action. The world's dangers take visual priority over the player's position, because the world does not care about the player, and the visual hierarchy should confirm that.

---

### 3.6 Shape Language Summary Table

| Element | Primary shape primitive | Terminal geometry | Negative space | Aspect ratio | Grid alignment | Pillar served |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Apex predator | Convergent horizontal mass | Acute angles (<60 deg) | Low (15%) | Horizontal | None | P1, P2 |
| Ambush predator | Deceptive (mimics other category) | Hidden angular (revealed on strike) | Matches mimicked category | Matches mimicked category | None | P2 |
| Prey species | Dispersed vertical mass | Rounded (convex caps) | High (40%+) | Vertical/neutral | None | P2 |
| Scavenger | Flattened horizontal mass | Mixed (angular head, rounded body) | Moderate (25-30%) | Horizontal (flat) | None | P2 |
| Territorial defender | Planted vertical mass | Blunt (truncated flat) | Low (15-20%) | Vertical | None | P1, P2 |
| Player creature (base) | Compact bottom-heavy mass | Rounded (prey-adjacent) | Moderate (30-35%) | Neutral | None | P4 |
| Player creature (evolved) | Hybrid (varies by evolution path) | Mixed (shifts with adaptations) | Varies (tracks role shift) | Varies | None | P4 |
| Human structure | Euclidean rectilinear | Right angles (90 deg) | Low (varies by structure) | Varies | 8x8 sub-grid | P3 |
| Biological growth | Fractal branching | Rounded, tapering | Variable | Varies (follows substrate) | None (anti-grid) | P3, Cardinal Rule |
| Collision zone | Disrupted Euclidean | Softened right angles | Narrative (gaps tell stories) | Varies | Warped grid | P3, Cardinal Rule |
| UI elements | Borrowed from world (diegetic) | Matches surface material | N/A | N/A | Matches surface | P1, Uninvited Frame |

---

### 3.7 Shape Review Checklist

Before any sprite, tile, or UI element enters the asset pipeline, it must pass the following shape-level review. This checklist is applied BEFORE color, animation, or detail passes.

1. **Silhouette role test.** Render the asset as a solid black shape. Can a reviewer correctly identify its ecological category (or structural origin) without any other context? If no: the silhouette has failed, and no amount of color or animation will save it.

2. **Thumbnail legibility test.** Reduce the asset to 50% of its intended display size. Are the diagnostic shape features (terminal geometry, aspect ratio, negative space distribution) still readable? If no: the shape features are too subtle and must be exaggerated.

3. **Diagnostic ridge conflict test (creatures only).** Does the new creature's silhouette produce any contour within 2 pixels of the player creature's diagnostic ridge shape at any rotation? If yes: modify the new creature. The ridge is reserved.

4. **Grid compliance test (environment only).** Human-built tiles: do all structural lines align to the 8x8 sub-grid? Biological tiles: do any structural lines align to the 8x8 sub-grid (they should not)? Collision zone tiles: is the grid present but visibly disrupted?

5. **Cardinal Rule test.** For every non-trivial shape element in the asset, can the artist answer: "Why does this shape exist here?" If the answer involves aesthetics, pattern, or visual balance rather than ecological function, structural physics, or biological growth -- the shape violates the Cardinal Rule and must be reworked.

6. **Category purity test (creatures only).** Does the creature's terminal geometry, negative space ratio, and aspect ratio place it cleanly in one taxonomy category (or, for ambush predators and the player creature, in a documented hybrid)? If the shape signals are ambiguous or contradictory in a way not justified by the creature's ecological role, the silhouette is sending mixed signals and must be clarified.

7. **Layer separation test (full scene).** Place the asset in its intended parallax layer alongside other assets from that layer and adjacent layers. Does it read at the correct depth? Does it compete for attention with assets on other layers? Background assets should be visually simpler; foreground assets should partially occlude without confusing the gameplay layer.

---

## 4. Color System

Color in Rewild is not decoration. It is stratigraphy. Every pixel's hue, saturation, and value answers the question: *when did this surface last belong to humans, and how thoroughly has biology replaced it?* The Decay Gradient (Section 1) established this principle. This section turns it into a production-ready system -- a finite set of colors, organized into palette ramps, with explicit semantic roles, per-biome rules, game-state modulation, colorblind safety protocols, and UI integration.

**The color constraint:** Rewild uses a **28-color master palette** plus **4 reserved synthetic artifact colors** (32 total). Every pixel in the game is painted from this set. No gradients. No blending. No color that is not in the palette file. This constraint is not a limitation -- it is a design tool. A restricted palette forces the Decay Gradient to be readable: with only 28 colors available, the warm-to-cool succession cannot be muddied by intermediate hues. Every color is a word. Every word means something.

Artists load the palette file (`assets/art/palettes/rewild-master.pal`) into their editor and paint exclusively from it. If a color is not in the file, it does not exist in the game.

---

### 4.1 Master Palette

The 32-color master palette is organized into five functional groups. Each group is a ramp -- a sequence of colors ordered by value (dark to light) within a shared hue family. The ramp structure is the production tool: when an artist needs a darker or lighter version of a color, they move along the ramp, not into a color picker.

Every color has three identifiers: a **name** (human-readable, used in documentation and reviews), a **hex code** (used in the palette file and asset production), and a **semantic role** (what the color means in the world of Rewild).

---

#### 4.1.1 Human-Era Ramp (8 colors)

The warm, muted spectrum. These colors represent human civilization -- concrete, rust, dried earth, bleached surfaces, oxidized metal. They are the oldest visible layer in any scene. They get warmer and more muted as the human material gets older or deeper-buried.

**Hue range:** 20-45 degrees (red-orange to yellow-orange).
**Saturation range:** 15-35% (muted, never vivid).
**Characteristic:** Dusty, granular, chalky. These colors look like they have been sitting in diffuse light for decades. Nothing is shiny. Nothing is clean.

| # | Name | Hex | H/S/V | Semantic Role |
| -- | -- | -- | -- | -- |
| H1 | Foundation | `#2B2220` | 20/24/17 | Deepest buried human material. Basement floors, subterranean concrete, load-bearing walls in shadow. The oldest human color visible. |
| H2 | Rebar | `#4A3530` | 15/33/29 | Exposed structural steel, corroded metal, oxidized iron. The skeleton of human infrastructure. |
| H3 | Concrete | `#6B5B52` | 25/24/42 | Standard concrete surfaces. Walls, floors, ceilings, structural slabs. The most common human-era color -- the workhorse of ruin environments. |
| H4 | Mortar | `#8A7668` | 28/25/54 | Lighter concrete, grout lines, dried plaster, stucco. Exposed where surface coatings have worn away. |
| H5 | Rust | `#9B6B4A` | 25/52/61 | Active oxidation. Metal surfaces where moisture has been working. The warmest and most saturated color in the human ramp -- rust is the most visible sign that chemistry, not biology, is decomposing a surface. |
| H6 | Dust | `#B8A48E` | 32/23/72 | Surface dust, dried mud, accumulated sediment on horizontal planes. The color of time deposited as powder. |
| H7 | Bleach | `#D4C4AD` | 38/19/83 | Sun-bleached surfaces. Exterior concrete and stone that have faced direct light for decades. Warm but washed out. |
| H8 | Bone | `#E8DDD0` | 35/10/91 | The brightest human-era color. Exposed limestone, clean-broken concrete edges, bone-dry surfaces in direct light. Used sparingly -- most human surfaces are too dirty or colonized to reach this value. |

**Ramp usage rule:** In any given tile, the human-era ramp should span no more than 4-5 adjacent steps. A surface painted with H1 through H8 in a single tile is too wide a value range -- it implies a light source that does not exist. Typical ruin tiles use 3-4 colors from this ramp, with the darkest in shadow and the lightest at exposed edges.

---

#### 4.1.2 Biological Ramp (10 colors)

The cool, saturated spectrum. These colors represent living systems -- chlorophyll, moss, lichen, algae, fungal tissue, canopy shadow. They are the newest visible layer in any scene. They get cooler and more saturated as the biological growth is more recent or more active.

**Hue range:** 100-200 degrees (yellow-green through teal to blue-green).
**Saturation range:** 30-65% (noticeably more saturated than human-era colors).
**Characteristic:** Wet, layered, textured. These colors look alive. Even the darkest biological tones have a depth that the flat, chalky human-era colors lack.

| # | Name | Hex | H/S/V | Semantic Role |
| -- | -- | -- | -- | -- |
| B1 | Rootdark | `#1A2A1E` | 140/38/16 | Deep shadow under dense canopy. Root interiors. The darkest color in the biological spectrum -- used for cavities, root tunnels, and the underside of dense growth. |
| B2 | Peat | `#2D3B28` | 105/32/23 | Decomposing organic material. Forest floor, humus, wet leaf litter. The color of biology recycling itself. |
| B3 | Deepmoss | `#3A5438` | 130/34/33 | Established moss colonies in shade. Old growth on north-facing surfaces. The baseline dark-green of stable biological coverage. |
| B4 | Chlorophyll | `#4A7A48` | 118/41/48 | Active photosynthetic tissue. Leaf surfaces, living vine skin, healthy ground cover. The most common biological color -- the workhorse green of living environments. |
| B5 | Canopy | `#5D9B55` | 112/45/61 | Sunlit leaf surfaces. Upper canopy growth, young leaves in direct light. Brighter and warmer than Chlorophyll, indicating active growth in good light conditions. |
| B6 | Lichen | `#6B9B7A` | 150/32/61 | Lichen colonies, slow-growing surface coverings, aged biological crusts. Cooler and less saturated than true green -- lichen is biology but not vigorous biology. |
| B7 | Algae | `#4A8B7A` | 165/46/55 | Wet-surface biology. Algae films on flooded surfaces, moist concrete, water-edge growth. The hue shifts toward teal, signaling water proximity. |
| B8 | Teal | `#3A7B7B` | 180/52/48 | Deep aquatic biology. Submerged growth, deep-water algae, fungal colonies in flooded zones. The coolest green, approaching pure cyan. |
| B9 | Sporeglow | `#5AABA0` | 172/48/67 | Bioluminescent fungal tissue. Active spore-producing surfaces. The brightest cool tone in the biological ramp -- used for fungal light sources and the glow of living mycelium networks. |
| B10 | Newgrowth | `#8BC88A` | 119/32/78 | The newest, freshest growth. Sprout tips, unfurling fronds, new shoots breaking through surfaces. The brightest and most yellow-shifted green -- spring color, first colonization, the leading edge of biological takeover. |

**Ramp usage rule:** Biological colors follow the same 4-5 step maximum per tile. But unlike the human-era ramp (where value steps are roughly linear), the biological ramp allows jumps -- a Rootdark (B1) shadow can sit next to a Canopy (B5) highlight if the lighting justifies it, because real foliage produces high-contrast light/shadow transitions through canopy gaps. The key constraint is that cool tones (B7-B9) and warm tones (B4-B5) should not appear on the same biological surface without an ecological reason (e.g., a transition from dry moss to wet algae at a waterline).

---

#### 4.1.3 Transitional Colors (4 colors)

These colors occupy the hue space between the human-era and biological ramps. They represent the collision zone -- surfaces where neither human material nor biological growth is fully dominant. They are the Decay Gradient's midpoint.

| # | Name | Hex | H/S/V | Semantic Role |
| -- | -- | -- | -- | -- |
| T1 | Wetrust | `#5A4A38` | 35/38/35 | Rust staining on biological surfaces. The color of iron oxide leaching into organic material. Where metal meets moss. |
| T2 | Staingreen | `#5A6B48` | 85/34/42 | Biological staining on concrete. Algae discoloration, moss shadow on human surfaces. The first visible sign that biology is winning. |
| T3 | Mudite | `#7A7A5A` | 60/26/48 | The dead center of the Decay Gradient. Neither warm nor cool, neither human nor biological. Mud, sediment, mixed debris. The color of erosion products -- ground-up human material mixed with decomposing organic matter. |
| T4 | Patina | `#6B8B6B` | 120/23/55 | Oxidized copper and bronze. Verdigris. The one transitional color that is specifically metallic, specifically human-origin, but reads as green. The visual embodiment of chemistry mimicking biology. |

**Transitional usage rule:** These colors appear exclusively in collision zone tiles (Section 3.3.3). They bridge the human-era and biological ramps within a single tile or across adjacent tiles. A collision zone tile that uses only human-era or only biological colors is wrong -- it is not showing the collision. At least one transitional color must be present in every collision zone tile.

---

#### 4.1.4 Atmospheric Colors (6 colors)

Sky, water, deep shadow, and the value extremes that no object ramp reaches. These colors define the environment's container -- the space in which the human-era and biological elements exist.

| # | Name | Hex | H/S/V | Semantic Role |
| -- | -- | -- | -- | -- |
| A1 | Void | `#0E0E12` | 240/15/7 | The deepest shadow in the game. Interior spaces with no light source. Cavern depths. The inside of collapsed structures. Not pure black -- the faint blue cast prevents it from reading as "missing pixel." |
| A2 | Nightblue | `#1A2030` | 220/44/19 | Deep twilight sky. The ambient color of darkness in Rewild -- darkness is always slightly blue, never neutral gray. |
| A3 | Duskgray | `#3A4050` | 220/20/31 | Overcast sky, rain ambient, fog base color for most biomes. The neutral atmosphere color. |
| A4 | Skywash | `#7090A8` | 205/33/66 | Clear daytime sky reflected on surfaces. Water surface color in daylight. The brightest cool neutral. |
| A5 | Cloudlight | `#C0C8D0` | 210/8/82 | Overcast light source color. The diffuse white of a cloudy sky -- never pure white, always faintly blue. |
| A6 | Shaftlight | `#E8E0C8` | 43/14/91 | Direct warm light. Light shafts in ruins, golden hour illumination, the brightest warm light that enters interior spaces. The only near-white with a warm cast. |

**Atmospheric usage rule:** A1 and A2 are used for environmental shadow and negative space -- never for object surfaces. A5 and A6 are used for light effects and the brightest highlights -- never as fill colors for surfaces. A3 and A4 bridge the gap and can appear as both environmental fill (sky, water) and reflected light on surfaces.

---

#### 4.1.5 Synthetic Artifact Colors (4 colors)

The chromatic archaeology palette. These colors exist nowhere in the biological world of Rewild and nowhere in the human-era ramp. They are the ghost-colors of industrial humanity -- the synthetic pigments and screen-emitted hues that nature does not produce. They appear ONLY on human-made objects found in ruins, and their presence is itself a narrative signal: *a human made this.*

**Critical restriction:** These four colors are forbidden outside of Discovery/ruin contexts. No biological organism, no terrain surface, no atmospheric effect, and no UI element may use these colors. They are reserved for artifacts. If a player sees Synthetic Yellow, they know with absolute certainty: this object is human-made. This certainty is the foundation of the chromatic archaeology system.

| # | Name | Hex | H/S/V | Semantic Role |
| -- | -- | -- | -- | -- |
| S1 | Signal Yellow | `#C8A830` | 48/76/78 | Warning signage, safety markings, caution tape, hazard indicators. The most immediately recognizable human color -- no organism in Rewild is this yellow. Desaturated from its original factory-bright hue by decades of light exposure, but still unmistakably synthetic. |
| S2 | Plastic Orange | `#B87030` | 28/74/72 | Consumer product housings, traffic cones, safety equipment, industrial casings. The color of injection-molded plastic. Faded but holding its hue because plastic pigments resist UV degradation longer than paint. |
| S3 | Screen Cyan | `#40A0B8` | 190/65/72 | CRT/LCD screen remnants, indicator lights (dead but color-preserved behind glass), medical displays, emergency lighting panels. The color of light that was once emitted, now frozen as pigment in shattered glass and degraded phosphors. |
| S4 | Circuit Trace | `#30A048` | 145/70/63 | PCB substrates, circuit board traces visible in broken electronics, the specific green of solder-masked fiberglass. This is the one synthetic color that overlaps with the biological hue range -- and that overlap is the point. It creates a moment of ambiguity: "Is that a plant, or is that a motherboard?" The answer tells the player where they are in the ruin. |

**Synthetic color usage rules:**

1. **Desaturation by depth.** Synthetic colors near the surface of a ruin (close to biological colonization, exposed to light) are desaturated by 20-30% from the hex values above. Synthetic colors deep in a ruin (protected from light and biology) retain their full saturation. The deeper you go, the more vivid the human colors become. This is chromatic archaeology -- you are digging into the past, and the past is more colorful than the present.

2. **Never at full value.** Even at maximum depth, synthetic colors are rendered at 85-90% of the value listed above. They are old. They are not factory-fresh. The slight dimming prevents them from overpowering the biological palette, which must remain the visually dominant system in every scene.

3. **Maximum one synthetic color per screen.** Two human artifacts with different synthetic colors on the same screen creates visual clutter and dilutes the archaeological signal. One artifact, one color, one story. If a ruin room contains multiple human objects, they share a single synthetic color (a server room is all Screen Cyan; a maintenance corridor is all Signal Yellow).

4. **S4 (Circuit Trace) is the ambiguity color.** It is used specifically in scenes where the boundary between biological and technological is thematically important -- server farms overgrown with real plants, fungal colonies colonizing circuit boards. Its overlap with the biological green range is a designed confusion that rewards close inspection.

---

### 4.2 Palette Ramp Reference

For production use. Each ramp is shown as a value progression from darkest to lightest. Artists select colors by moving along a ramp, never across ramps (except in collision zone tiles, where transitional colors bridge the two primary ramps).

```
HUMAN-ERA RAMP (warm, muted)
H1 ---- H2 ---- H3 ---- H4 ---- H5 ---- H6 ---- H7 ---- H8
#2B2220  #4A3530  #6B5B52  #8A7668  #9B6B4A  #B8A48E  #D4C4AD  #E8DDD0
[17V]    [29V]    [42V]    [54V]    [61V]    [72V]    [83V]    [91V]

BIOLOGICAL RAMP (cool, saturated)
B1 ---- B2 ---- B3 ---- B4 ---- B5 ---- B6 ---- B7 ---- B8 ---- B9 ---- B10
#1A2A1E  #2D3B28  #3A5438  #4A7A48  #5D9B55  #6B9B7A  #4A8B7A  #3A7B7B  #5AABA0  #8BC88A
[16V]    [23V]    [33V]    [48V]    [61V]    [61V]    [55V]    [48V]    [67V]    [78V]

TRANSITIONAL RAMP (collision zone)
T1 ---- T2 ---- T3 ---- T4
#5A4A38  #5A6B48  #7A7A5A  #6B8B6B
[35V]    [42V]    [48V]    [55V]

ATMOSPHERIC RAMP (environment container)
A1 ---- A2 ---- A3 ---- A4 ---- A5 ---- A6
#0E0E12  #1A2030  #3A4050  #7090A8  #C0C8D0  #E8E0C8
[7V]     [19V]    [31V]    [66V]    [82V]    [91V]

SYNTHETIC ARTIFACTS (ruins only -- NEVER in biological contexts)
S1 ---- S2 ---- S3 ---- S4
#C8A830  #B87030  #40A0B8  #30A048
Signal   Plastic  Screen   Circuit
Yellow   Orange   Cyan     Trace
```

---

### 4.3 Per-Biome Color Temperature Rules

Section 3.3.4 defined biome shape differentiation. This section defines how the master palette is filtered, weighted, and temperature-shifted for each biome archetype. The same 32 colors are used everywhere -- biome identity is created by which colors dominate, which recede, and how the warm-to-cool ratio shifts.

**Master principle:** The ratio of human-era colors to biological colors on any given screen tells the player the biome's decay state. The color temperature of the biological ramp tells the player the biome's moisture and light conditions. Shape tells the player *what happened here*; color tells the player *when* and *how wet*.

---

#### 4.3.1 Fresh Ruin

**Decay state:** Early. Human infrastructure is mostly intact. Biological colonization is sparse -- pioneer species only.

**Color temperature:** Warm-dominant. The human-era ramp (H1-H8) provides 70-80% of the screen's pixel coverage. Biological colors are present but limited to the lower-saturation end of the ramp (B2-B4). No teal or aquatic tones unless water is physically present.

**Dominant colors:** H3 (Concrete), H4 (Mortar), H6 (Dust), H7 (Bleach). The world looks like an abandoned building that has been empty for a decade, not a century.

**Biological accent:** B3 (Deepmoss) and B4 (Chlorophyll) appear at structural joints, drainage points, and cracks -- wherever moisture collects. Coverage is sparse: 15-25% of tile area. The green reads as intrusion, not dominance.

**Transitional colors:** Minimal. T2 (Staingreen) at moisture stains on concrete. T1 (Wetrust) where exposed rebar meets rain.

**Atmospheric:** A5 (Cloudlight) and A6 (Shaftlight) for exterior-exposed surfaces. Interiors use A3 (Duskgray) as ambient.

**Synthetic artifacts:** Most common in this biome. Artifacts are closest to the surface and most recently exposed. S1 (Signal Yellow) and S2 (Plastic Orange) at 80-90% of listed saturation -- still relatively vivid.

**Overall temperature read:** A fresh ruin screen reads as warm-neutral. The ochres and concretes dominate. The eye registers "building" before "ecosystem." This is the warmest biome in the game.

---

#### 4.3.2 Mid-Decay (Collision Dominant)

**Decay state:** Advanced. Human infrastructure is structurally compromised. Biological growth and human material share roughly equal screen presence. This is the visual heart of the Decay Gradient -- both temporal poles are visible simultaneously.

**Color temperature:** Neutral, trending cool. Human-era and biological colors share screen coverage at roughly 40-40%, with transitional colors filling the remaining 20%. The eye cannot decide if this is a building or a forest, which is exactly the intended read.

**Dominant colors:** H3 (Concrete) and H5 (Rust) for the human layer. B3 (Deepmoss), B4 (Chlorophyll), and B5 (Canopy) for the biological layer. T2 (Staingreen) and T3 (Mudite) at every boundary between the two.

**Biological accent:** No longer an accent -- a co-dominant system. Biological coverage is 35-50% of tile area. The biological ramp is used at its full warm-green range (B3-B5), with B6 (Lichen) on older, more established surfaces.

**Transitional colors:** Maximum use. Every collision zone tile (which is most tiles in this biome) uses at least one transitional color. T3 (Mudite) is the most common -- the dead-center color that belongs to neither world.

**Atmospheric:** Mixed. Exterior surfaces receive both warm (A6) and cool (A4) light depending on canopy coverage. Interiors are A3 (Duskgray) with A6 (Shaftlight) through structural breaches.

**Synthetic artifacts:** Present but deeper. Artifacts are partially buried or colonized. S1-S4 at 60-70% of listed saturation. They require closer inspection to distinguish from biological growth, especially S4 (Circuit Trace).

**Overall temperature read:** A mid-decay screen reads as the Decay Gradient in equilibrium. Warm and cool compete. The collision is visible in every tile. This biome should feel like the moment the scales are tipping -- nature is about to win but has not quite won yet.

---

#### 4.3.3 Fully Reclaimed (Deep Reclamation)

**Decay state:** Near-terminal. Biological systems dominate. Human infrastructure survives only as a skeletal framework beneath overwhelming organic growth.

**Color temperature:** Cool-dominant. The biological ramp provides 65-80% of screen pixel coverage. Human-era colors are visible only as glimpses through gaps in foliage, exposed at structural fracture points, or visible in cross-sections where erosion has cut through the biological layer.

**Dominant colors:** B3 (Deepmoss), B4 (Chlorophyll), B5 (Canopy), B1 (Rootdark) for shadow. Human colors reduced to H1 (Foundation) and H3 (Concrete) at structural remnants, plus H5 (Rust) at exposed metal.

**Biological accent:** The biological ramp IS the primary palette. The full range B1-B6 is in use. B9 (Sporeglow) and B10 (Newgrowth) appear where biological activity is most vigorous -- growing tips, fungal colonies, recent colonization fronts.

**Transitional colors:** Reduced. The collision is mostly over. T4 (Patina) on surviving metal surfaces. T2 (Staingreen) where concrete is still partially visible.

**Atmospheric:** Cool-dominant. A2 (Nightblue) in canopy shadow. A4 (Skywash) reflected off wet surfaces. A6 (Shaftlight) only where significant canopy gaps allow direct light -- these warm light shafts become compositional anchors in an otherwise cool scene.

**Synthetic artifacts:** Rare and deep. Only found in the most structurally intact interior spaces. S1-S4 at 50-60% saturation. Their vivid warmth is shocking against the cool biological background, which is the intended narrative effect: the deeper you dig, the more color you find, and the stranger that color feels against the green world.

**Overall temperature read:** A fully reclaimed screen reads as cool green with warm punctuation. The eye registers "forest" or "jungle" first, then notices the straight lines and right angles of the surviving human skeleton beneath. This is what the Decay Gradient looks like when biology has almost finished its work.

---

#### 4.3.4 Fungal / Subterranean

**Decay state:** Variable, but always underground. Human infrastructure ranges from intact (sealed basements, server rooms) to collapsed (cave-ins, flooded tunnels). The unique factor is the absence of photosynthetic biology -- the living systems here are fungal, not plant-based.

**Color temperature:** Cool, shifting toward teal and blue-green. The fungal biome uses the cool end of the biological ramp (B7-B9) where other biomes use the warm end (B4-B5). This temperature difference is the primary visual differentiator: fungal zones feel colder than surface biomes even when the palette's value range is similar.

**Dominant colors:** B7 (Algae), B8 (Teal), B9 (Sporeglow) for fungal tissue. H1 (Foundation), H2 (Rebar) for deep structural elements. A1 (Void) and A2 (Nightblue) for the pervasive darkness.

**Biological accent:** B9 (Sporeglow) is the primary light source color in fungal zones. Bioluminescent fungal colonies provide the only illumination. The light they produce is cool teal-green, which means every surface is lit in cool tones regardless of its material. H3 (Concrete) lit by Sporeglow reads as a muted green-gray rather than its natural warm tone. The fungal light transforms the human-era ramp, shifting it cool.

**Transitional colors:** T1 (Wetrust) where metal contacts moisture. T4 (Patina) on copper and bronze fittings. The other transitional colors are too warm-green to be relevant in a lightless zone.

**Atmospheric:** A1 (Void) is the dominant fill. A2 (Nightblue) for areas at the edge of bioluminescent range. No A5 or A6 -- there is no sky and no warm light source.

**Synthetic artifacts:** Maximum preservation. Sealed underground spaces have the least UV degradation and the least biological colonization. S1-S4 at 85-95% of listed saturation. In deep server rooms, Screen Cyan (S3) on dead monitors is nearly as vivid as it was when the power went off. These are the most chromatically intense moments in the game, and they feel alien -- a burst of synthetic color in a world that has forgotten what color television was.

**Overall temperature read:** A fungal zone screen reads as teal-on-black with warm structural undertones. The bioluminescence creates pools of cool light separated by deep shadow. The eye registers "cave" and "alien" simultaneously. The temperature is the coldest in the game.

---

#### 4.3.5 Thermal / Industrial

**Decay state:** Severe but unusual. Heat has not merely decayed human infrastructure -- it has deformed it. Metals are warped. Concrete is spalled and bubbled. Plastics have melted. The decay is not biological but thermochemical.

**Color temperature:** Warm, but a different warm than the human-era ramp. The warmth here is active -- radiant heat, not archaeological warmth. The biological colors in thermal zones are also uniquely warm: heat-adapted organisms use the yellow-green end of the spectrum (B4-B5, shifted toward yellow) rather than the cool greens of shade-adapted biomes.

**Dominant colors:** H2 (Rebar) and H5 (Rust) for heat-deformed metal -- these are the most saturated human-era colors and they dominate because heat accelerates oxidation. B4 (Chlorophyll) shifted warm (heat-adapted variants sit at hue 105-110 rather than the standard 118). A6 (Shaftlight) as ambient -- the air itself carries warm light from residual heat sources.

**Unique palette behavior -- Thermal glow:** This biome introduces a rendering rule unique to its environment: surfaces near active heat sources (geothermal vents colonized by thermophilic organisms, still-hot server exhaust systems) receive a 1-pixel highlight line along their heat-facing edge using H5 (Rust) at +15% value. This highlight is not a new color -- it is an existing palette color used at a higher brightness position than its ramp position normally allows. It is the only context in which a palette color may exceed its listed value, and it is justified by the physical reality of radiant heat illumination.

**Biological accent:** Heat-adapted organisms are visually distinct. They use B4 and B5 at their warmest interpretations, plus angular shapes (per Section 3.3.4). The biological presence feels less like plant growth and more like mineral crystallization -- hard, geometric, warm-colored. This is the one biome where biological colors approach the warmth of human-era colors, compressing the Decay Gradient. The compression is thematically appropriate: in a place where everything is hot, the temperature distinction between past and present collapses.

**Atmospheric:** A6 (Shaftlight) as the dominant ambient -- warm, directional from heat sources. A1 (Void) in shadow, but shadows are shorter and softer than in fungal zones because ambient heat glow fills them partially.

**Synthetic artifacts:** Heat-damaged. S2 (Plastic Orange) is melted and distorted -- the shapes are wrong even though the color survives. S1 (Signal Yellow) is heat-faded to near-H6 (Dust), making it hard to distinguish from natural warm surfaces. S3 (Screen Cyan) is the most conspicuous synthetic color here because the thermal palette is so warm that any cool tone is visually alien.

**Overall temperature read:** A thermal zone screen reads as amber-to-rust with angular green-yellow accents. The warmth is oppressive rather than comforting (contrast with Shelter, which is warm but enclosed and safe). The eye registers "hot" and "industrial" -- metal, heat, pressure.

---

### 4.4 Game-State Color Modulation

Section 2 defined the emotional target and palette rules for each of the 8 game states. This section translates those rules into concrete color operations that an artist or a Godot shader can execute against the master palette. Each game state applies a specific, reproducible transformation to the master palette.

**Implementation note:** These modulations are not post-processing effects. They are palette-swap operations. For each game state, a modified copy of the master palette is generated (or hand-tuned from the base). The game loads the appropriate palette variant when transitioning between states. The pixel art itself does not change -- only the palette it references changes. This is the classic palette-cycling technique, adapted for Rewild's state system.

---

#### 4.4.1 Exploration (Baseline)

**Palette operation:** None. Exploration uses the master palette at its listed values. All other states are defined as departures from this baseline.

---

#### 4.4.2 Tension

**Palette operation:** Warm desaturation + cool shift + value compression.

| Ramp | Saturation change | Hue shift | Value change |
| -- | -- | -- | -- |
| Human-era (H1-H8) | -15 to -20% absolute | +5 degrees toward blue | Max value capped at 75% (H7 and H8 darken) |
| Biological (B1-B10) | -10% absolute | +10 degrees toward blue | Dark values unchanged, bright values -15% |
| Transitional (T1-T4) | -15% absolute | +5 degrees toward blue | -10% uniform |
| Atmospheric (A1-A6) | Unchanged | Unchanged | A5 and A6 darken by 20-30% |
| Synthetic (S1-S4) | -25% absolute | Unchanged | -15% uniform |

**Net effect:** The palette becomes cooler, darker, and more compressed. The Decay Gradient's warm/cool distinction is still visible but muted. The world feels like late afternoon sliding into dusk.

**Player creature exception:** The player creature's most identifiable color (the diagnostic ridge's highlight color) retains its Exploration-baseline saturation. One marking stays vivid. This is the prey-recognition principle: the creature's conspecific signal remains at full strength even as the environment desaturates.

---

#### 4.4.3 Pursuit

**Palette operation:** Extreme value reduction + functional color isolation.

The palette collapses to effectively 4 functional colors:

| Function | Source color | Pursuit-state value | Everything else |
| -- | -- | -- | -- |
| Player creature | Full creature palette | Retained at Exploration values | -- |
| Traversable surfaces | H3 (Concrete) or B4 (Chlorophyll), whichever is the current biome's dominant platform color | Boosted by +20% value (surfaces the player can land on become the brightest environmental element) | -- |
| Predator silhouette | Predator's darkest body color | Retained at Exploration value (reads as dark mass against dimmed background) | -- |
| Everything else | All remaining palette entries | Reduced to 15-25% of Exploration value (near-black) | Functional collapse |

**Net effect:** The screen becomes a two-tone system: bright traversable surfaces and near-black everything else, with the player and predator as the only readable sprites. Color information is gone. Only value survives. This is tunnel vision rendered as a palette swap.

---

#### 4.4.4 Observation

**Palette operation:** Subject enhancement + player suppression + peripheral dimming.

| Element | Palette treatment |
| -- | -- |
| Observed creature | Full Exploration-baseline palette. No modification. This is the creature's "true colors" -- accurate for identification and study. |
| Player creature | All colors reduced by -20% saturation and -15% value. The player becomes camouflage -- muted, receding, unimportant. |
| Environment within 3 tiles of subject | Exploration-baseline palette. The subject's immediate habitat is rendered accurately. |
| Environment beyond 3 tiles of subject | Value reduced by -10 to -15%. Saturation reduced by -10%. The periphery dims, creating the diegetic focus vignette through palette, not post-processing. |

**Net effect:** The observed creature is the most colorful thing on screen. The player is the least. The environment gradually desaturates from the subject outward. The eye is pulled to the subject without any UI highlighting.

---

#### 4.4.5 Discovery

**Palette operation:** Vertical temperature split + synthetic color activation.

| Ramp | Top of screen (ceiling) | Bottom of screen (floor) |
| -- | -- | -- |
| Human-era (H1-H8) | -10% saturation (biological colonization from above has stained the warm tones) | Full Exploration values (deeper = more protected = more preserved) |
| Biological (B1-B10) | Full Exploration values (biology enters from above) | -20% saturation, -15% value (less biological reach at depth) |
| Transitional (T1-T4) | Concentrated in the mid-zone where the two temperature poles meet | Concentrated in the mid-zone |
| Synthetic (S1-S4) | Absent (too much biological coverage) | Active. Full desaturation-by-depth rules apply (Section 4.1.5) |
| Atmospheric | A6 (Shaftlight) in ceiling breaches. A2 (Nightblue) as interior ambient | A1 (Void) in deepest interiors. A6 where light shafts reach the floor |

**Net effect:** Reading the screen from top to bottom tells the chronology. Cool, saturated biological greens at the top (where nature entered). Warm, muted human-era tones at the bottom (where artifacts are buried). Synthetic artifact colors punctuate the lower strata like fossils. The Decay Gradient becomes literal geology.

---

#### 4.4.6 Shelter

**Palette operation:** Full warm compression + cool exclusion.

The palette narrows to 8-12 colors, all within the warm range:

| Active colors | Source | Shelter-state modification |
| -- | -- | -- |
| H3, H4, H5, H6, H7 | Human-era ramp | Saturation +10%, value +10-15%. Concrete becomes amber-tinted. Rust becomes warm copper. Dust becomes golden. The same tiles that read as melancholic in ruins read as protective here. |
| T1 | Transitional | Warmed further: hue shift -10 degrees (toward orange). Reads as "den material." |
| A6 | Atmospheric | Used as the dominant light source. At full listed value. The brightest element on screen. |
| A1 | Atmospheric | Used for any exterior visible through an opening. Maintains Exploration or current-state exterior values, creating the inside/outside temperature contrast. |
| B3 or B9 | Biological | The single cool accent. One of these (depending on biome) represents biological light or material that has been incorporated into the shelter. Only 1-2 tiles' worth. The green reads as "life nearby" without breaking the warm enclosure. |

**All other palette entries:** Unused. The biological ramp, the synthetic colors, and the bright atmospheric tones are absent. The palette inversion (Section 2.6) is achieved by removing cool colors entirely, not by recoloring them.

**Player creature:** Rendered using the shelter's warm subset. The creature's sprite palette shifts to match the ambient temperature. For the first time, the creature and the environment are the same warmth. It belongs.

---

#### 4.4.7 Weather Modulation

Weather is a multiplier on the current game state, not its own state. Each weather type applies an additional palette operation on top of whatever game-state palette is currently active.

| Weather | Palette operation |
| -- | -- |
| **Rain** | All colors: hue +10 degrees toward blue, saturation -15%, value -10%. Maximum screen value capped at 65%. The world goes blue-gray. Applied uniformly -- objects are not individually recolored; the "light" changes. |
| **Storm** | Rain palette + periodic lightning frames. Lightning frame: all values snap to either 10% (shadows) or 90% (lit surfaces) for 1 frame, then return to rain palette over 2 frames. Between flashes, ambient value drops an additional -20% below rain baseline. |
| **Wind** | Saturation -10% uniform. No hue shift. No value change. Wind is kinetic, not chromatic. The palette barely changes; the animation does the work. |
| **Fog** | All parallax layers beyond the second collapse to a single fill color: biome-dependent (A3 Duskgray for most biomes, A4 Skywash for highlands, T3 Mudite for thermal zones). Foreground palette is unchanged. The effect is spatial, not tonal -- depth is removed, not color. |

**Weather + game state interaction:** Weather modulation is applied AFTER game-state modulation. The operations stack. Rain during Tension means the Tension palette (already cooled and compressed) is further cooled and compressed by rain. Rain during Shelter means the Shelter palette (warm and compressed) receives the rain modulation, but only on exterior-visible elements -- the shelter interior is protected from weather palette changes. The shelter's warmth is literal: it keeps the weather's color out.

---

#### 4.4.8 Evolution Color Absorption

When the player creature evolves, the new adaptation absorbs colors from the biome that triggered it. This is a permanent palette addition to the creature's sprite, not a temporary modulation.

**Color absorption rules:**

| Biome trigger | Colors absorbed into creature sprite | Placement on creature |
| -- | -- | -- |
| Fresh ruin | H5 (Rust), H6 (Dust) | Armored surfaces, carapace edges. The creature carries the warm tones of human infrastructure. |
| Mid-decay | T2 (Staingreen), T3 (Mudite) | Body core, transitional surfaces between old and new tissue. The collision zone colors. |
| Fully reclaimed | B4 (Chlorophyll), B5 (Canopy) | Membrane surfaces, flexible tissue. The creature carries forest green. |
| Fungal / subterranean | B8 (Teal), B9 (Sporeglow) | Bioluminescent markings, sensory organs. The creature carries fungal light. |
| Thermal / industrial | H5 (Rust), B4 shifted warm (hue 105) | Radiator structures, heat-adapted tissue. The creature carries amber-heat. |
| Aquatic / flooded | B7 (Algae), A4 (Skywash) | Gill structures, aquatic membranes. The creature carries water-blue-green. |

**Cumulative effect:** A late-game creature that has evolved across multiple biomes becomes a walking palette sampler -- carrying Rust from the ruins, Teal from the fungal caverns, Chlorophyll from the reclaimed zones. Its body IS the Decay Gradient in reverse: biological life that has absorbed the colors of every environment it survived. The creature's sprite palette tells its history.

**Maximum creature palette:** The player creature sprite uses no more than 8 colors at any given evolution state (base form: 4 colors; each evolution adds 1-2 colors from the biome, while the base colors are retained). This 8-color maximum ensures the creature remains readable at small scales -- too many colors on a 16x16 to 32x32 sprite becomes visual noise.

---

### 4.5 Semantic Color Vocabulary

This section defines what colors communicate to the player. It is the translation layer between the palette (what colors exist) and the player's understanding (what colors mean). Every semantic category maps to specific palette entries with no overlap between categories that could cause confusion.

---

#### 4.5.1 Danger / Threat

**Primary danger signal:** Silhouette, not color. Per Section 3.2, predators are identified by shape (convergent, angular terminals, dense mass). Color is the secondary signal.

**Color role in danger:** Danger is communicated through the *absence* of color rather than the presence of a specific danger color. The Tension and Pursuit palette operations (Sections 4.4.2 and 4.4.3) strip color from the environment, creating a visual austerity that the player learns to associate with threat. The player does not see red and think "danger." The player sees color draining from the world and thinks "something is wrong."

**Specific danger signals:**

| Signal | Color mechanism | Why it works |
| -- | -- | -- |
| Predator proximity | Environment desaturates (Tension palette). Player creature's diagnostic marking retains saturation. The contrast between the muted world and the vivid marking IS the danger signal. | The player reads danger in the relationship between their creature's color and the environment's color, not in any single color. |
| Active threat (pursuit) | Value collapse. Everything non-essential goes near-black. Traversable surfaces brighten. The world becomes a path-or-void binary. | The player reads danger as the loss of environmental information. The less they can see, the more danger they are in. |
| Territorial boundary | No palette change at the boundary itself. The territorial defender's silhouette (vertical, symmetrical, blunt -- Section 3.2.5) is the warning. Its colors are whatever the biome provides. | Territorial danger is spatial, not chromatic. The shape says "do not enter." The color says nothing. |
| Ambush reveal | The ambush predator's strike animation produces a 1-2 frame flash of angular silhouette (Section 3.2.2). No color change on the predator itself. The shock is shape-based. | The player learns to fear shape-changes, not color-changes. This trains the right instinct: watch for movement, not for red. |

**What Rewild does NOT do:** There is no red danger indicator. No red glow on threats. No red screen-edge overlay when health is low. Red-as-danger is a convention that privileges the player -- it is the game telling you "this is bad." In Rewild, the world does not tell you anything. You observe and decide. The color system supports observation, not notification.

---

#### 4.5.2 Safety / Traversability

**Primary traversability signal:** Flat-top contour (Section 3.4.1). Any surface with 3+ consecutive horizontal pixels at the same height is a platform. Shape is the first-order information.

**Color role in traversability:** Value contrast is the secondary signal. Traversable surfaces are rendered at the upper end of whatever ramp dominates the current biome -- H6-H7 in ruins, B4-B5 in reclaimed zones. Non-traversable surfaces (too steep, too fragile, too overgrown) are rendered at the lower end. The player learns: brighter surfaces support weight.

| Surface type | Color treatment |
| -- | -- |
| Stable platform (human-built, intact) | H4-H7 range. The highest value human-era colors. The flat, predictable geometry is matched by the flat, predictable color. |
| Stable platform (biological, established) | B4-B5 range. Root bridges, thick vine lattices, compacted organic surfaces. Medium-high value, moderate saturation. |
| Unstable platform (human-built, decaying) | H3-H4 range with T2-T3 transitional staining. Darker than stable platforms. The value drop signals "this holds, but maybe not for long." |
| Non-traversable surface | B1-B2 (dark biological) or H1-H2 (dark structural) depending on biome. The lowest value colors. Dark = do not step here. |
| Shelter entrance | A6 (Shaftlight) visible from the interior. The warm glow leaking from a shelter entrance is the strongest navigational color signal in the game. It says "safety is here" without a UI marker. |

**During Pursuit:** Traversable surface signaling overrides all other color rules. Platforms boost to +20% value above their current-state values. This is the one context where color serves pure gameplay function over ecological logic -- and it is justified diegetically as the tunnel-vision effect (Section 2.3), where the fleeing creature's visual system highlights escape routes.

---

#### 4.5.3 Narrative Significance (Human Artifacts)

**Primary signal:** Synthetic artifact colors (S1-S4). The presence of any synthetic color is a narrative flag: *a human made this, and you are in a place where humans once lived.*

**Semantic density:** The specific synthetic color tells the player what kind of human space they are in:

| Synthetic color | Narrative meaning | Human space type |
| -- | -- | -- |
| S1 (Signal Yellow) | Warning, caution, safety instruction. Humans labeled this place as dangerous or restricted. The warning is still valid -- the danger may be structural, chemical, or ecological. | Industrial areas, maintenance corridors, mechanical rooms, hazardous material storage |
| S2 (Plastic Orange) | Consumer, civilian, everyday. This was a space where humans lived or worked in routine conditions. The orange of everyday objects -- not dramatic, not cautionary. | Offices, residences, commercial spaces, schools, hospitals |
| S3 (Screen Cyan) | Information, computation, communication. Humans processed data here. The cyan of screens implies a concentration of technology. | Server rooms, control centers, communication hubs, medical imaging suites |
| S4 (Circuit Trace) | Technological substrate. The deepest human layer -- not the objects humans used, but the systems humans built into the walls. Wiring, circuitry, infrastructure. | Behind walls, under floors, inside machinery, the hidden technical anatomy of buildings |

**Interaction:** Synthetic-colored objects are often the targets of the player's observation mechanic (Section 2.4). Studying a human artifact closely may reveal environmental storytelling: a faded warning sign, a partially legible screen, a circuit board configuration that implies the room's function. The color draws the player's eye to the artifact. The artifact rewards close attention with narrative information.

---

#### 4.5.4 Ecological Information

**Biome health:** The saturation of the biological ramp communicates biome vitality. A biome where B4-B5 are at full saturation is healthy and active. A biome where the same colors are desaturated by 15-20% is stressed -- drought, contamination, predator overpopulation, or ecological disruption.

**Creature dietary role:** Color temperature on creatures correlates with ecological role. This is not a strict rule but a tendency that the player internalizes over time:

| Creature type | Color tendency | Why |
| -- | -- | -- |
| Herbivore / filter-feeder | Cool-neutral (B4-B6 derivatives). Colors match the vegetation they consume. | Camouflage in plant environments. They are part of the biological ramp. |
| Predator (active) | Warm-neutral to warm (H3-H5 derivatives with B3 accents). Slightly warmer than their prey. | Many real predators carry warm tones (tawny, amber, umber) that break up their silhouette in mixed environments. Rewild follows this principle. |
| Scavenger | Mudite-adjacent (T3 derivatives). Colors match the debris and sediment they feed on. | Scavengers are transitional creatures, ecologically. Their colors sit in the transitional palette. |
| Fungal symbiont | Teal-to-blue (B7-B9 derivatives). Colors match the fungal systems they are associated with. | These creatures have co-evolved with the fungal biome and carry its visual signature. |

**Biome transition indicator:** When crossing between biomes (Section 3.3.4, biome transition rule), the player sees the dominant biological color gradually shift from one biome's characteristic hue to another. The color gradient across 3-5 screens IS the biome transition -- no label, no boundary marker, just a slow walk through the spectrum from one green to another.

---

#### 4.5.5 Evolution-Related Colors

**Pre-evolution signal (evolution readiness):** When the player creature has accumulated sufficient biome exposure to trigger evolution potential, the relevant body region begins a bioluminescent pulse. This pulse uses the current biome's brightest biological color (B5 in reclaimed zones, B9 in fungal zones, H5 shifted warm in thermal zones) at +10% value, cycling over a 4-second period. The pulse adds no new color -- it brightens an existing one rhythmically.

**Evolution glow:** During the 2-3 second transformation (Section 2.8), the mutation site emits a bioluminescent glow. The glow color is biome-determined:

| Biome | Evolution glow color |
| -- | -- |
| Fresh ruin | H5 (Rust) at +15% value -- warm amber glow |
| Mid-decay | T4 (Patina) at +15% value -- verdigris glow |
| Fully reclaimed | B5 (Canopy) at +20% value -- bright green glow |
| Fungal / subterranean | B9 (Sporeglow) at +15% value -- teal-white glow |
| Thermal / industrial | H5 (Rust) at +25% value -- deep amber-orange glow |
| Aquatic / flooded | B7 (Algae) at +20% value -- blue-green glow |

**Post-evolution permanence:** After transformation, the absorbed biome colors (Section 4.4.8) become permanent parts of the creature's sprite palette. The glow fades. The colors remain. The creature's body is its history.

**Ghost silhouette color:** The 1-second ghost image of the creature's pre-evolution form (Section 2.8) is rendered using H6 (Dust) at 10-15% opacity. The old form becomes a ruin -- warm, muted, fading. The Decay Gradient applied to the player's own body.

---

### 4.6 Colorblind Safety

Rewild's semantic color system was designed to avoid sole reliance on hue discrimination. The shape-first, color-second hierarchy (Silhouette Ethology) means that the most critical information -- creature type, threat level, traversability, structural origin -- is carried by shape before color. However, several color-dependent systems require explicit backup signals for players with color vision deficiencies.

**Affected populations:**

| Type | Prevalence | Colors confused |
| -- | -- | -- |
| Protanopia/Protanomaly (red-weak) | ~6% of males | Red-green confusion. H5 (Rust) may be confused with B4 (Chlorophyll) and T2 (Staingreen). |
| Deuteranopia/Deuteranomaly (green-weak) | ~5% of males | Red-green confusion. Same pairings as protanopia with different severity curve. |
| Tritanopia/Tritanomaly (blue-weak) | ~0.01% | Blue-yellow confusion. B7-B9 (teal-blue biological) may be confused with S1 (Signal Yellow). A2-A4 (blue atmospheric) may merge with H6-H7 (warm light). |

---

#### 4.6.1 Critical Semantic Pairings at Risk

Each pairing below identifies two colors that carry different semantic meanings but may appear identical to colorblind players, and specifies the backup cue that preserves the distinction.

**Pairing 1: Rust (H5) vs. Chlorophyll (B4) -- Human vs. Biological**

- **Risk level:** HIGH. This is the core Decay Gradient distinction. If a player cannot distinguish warm human-era tones from cool biological tones, the stratigraphic reading of every scene is compromised.
- **Value backup:** H5 (Rust) has a value of 61. B4 (Chlorophyll) has a value of 48. The 13-point value difference is distinguishable in most protanopia/deuteranopia cases, but marginal. In production, ensure H5 is always used at its listed value or brighter, and B4 at its listed value or darker, to maintain the value gap.
- **Shape backup:** Human-era surfaces use Euclidean geometry (right angles, grid alignment). Biological surfaces use fractal geometry (no right angles, branching). The shape grammar (Section 3.3) makes the human-vs-biological distinction without any color at all. This is the primary backup and is sufficient on its own.
- **Texture backup:** Human-era surfaces have uniform stroke width. Biological surfaces have variable stroke width. This difference is visible at any color perception.

**Pairing 2: Staingreen (T2) vs. Chlorophyll (B4) -- Transitional vs. Biological**

- **Risk level:** MODERATE. Both carry green hues and may merge for red-green colorblind players. The distinction matters because T2 signals collision-zone surfaces (both human and biological present) while B4 signals pure biological coverage.
- **Value backup:** T2 has a value of 42. B4 has a value of 48. The gap is only 6 points -- insufficient as a backup.
- **Shape backup:** T2 appears exclusively on collision zone tiles, which contain visible disrupted human geometry (Section 3.3.3). B4 appears on purely biological tiles, which contain no straight lines. The presence or absence of disrupted grid geometry is the reliable distinction.
- **Saturation backup:** T2 has a saturation of 34. B4 has a saturation of 41. The saturation difference is perceptible even when hue is not.

**Pairing 3: Traversable (bright) vs. Non-traversable (dark) surfaces**

- **Risk level:** LOW. Traversability is communicated primarily through value contrast, not hue. Bright surfaces = safe to stand on. Dark surfaces = do not step here. This distinction is fully colorblind-safe because it relies on lightness, not hue.
- **Shape backup:** Flat-top contour (3+ horizontal pixels) for traversable surfaces. No flat runs for non-traversable surfaces. Fully shape-based.

**Pairing 4: Synthetic artifacts (S1-S4) vs. biological/environmental colors**

- **Risk level:** MODERATE for S4 (Circuit Trace) specifically. S4 (green, hue 145) overlaps with the biological green range and may not be distinguishable from B4-B5 for red-green colorblind players. S1 (Signal Yellow) and S2 (Plastic Orange) are high-saturation warm tones that remain distinguishable from the low-saturation human-era warm tones even without red-green discrimination. S3 (Screen Cyan) is in the blue range and is safe for protanopia/deuteranopia.
- **S4 backup:** Circuit Trace objects are always rendered with the uniform stroke width of human manufacturing (Section 3.3.1). Their geometry is gridded, right-angled, and regular -- visually distinct from the fractal biological growth they overlap with. A PCB substrate has parallel traces and regular spacing; a plant has branching and variable width. The distinction is geometric, not chromatic.
- **S1/S2 backup:** Signal Yellow and Plastic Orange objects are always human-manufactured shapes -- rectangular signage, cylindrical containers, smooth-surfaced housings. Their silhouette geometry (Euclidean, symmetrical) distinguishes them from any biological element regardless of color perception.

**Pairing 5: Evolution glow colors across biomes**

- **Risk level:** LOW. Evolution glow colors differ across biomes, but the player does not need to distinguish between them simultaneously -- you only evolve in one biome at a time. The glow's biome association is reinforced by the surrounding environment (you know which biome you are in because you are in it). The glow itself is communicated by the brightness pulse animation, not by its hue.

**Pairing 6: Sporeglow (B9) vs. Signal Yellow (S1) -- Tritanopia**

- **Risk level:** LOW (affects ~0.01% of players). B9 (teal, hue 172) and S1 (yellow, hue 48) may converge for tritanopic players. These colors never appear in the same biome context (B9 is fungal/subterranean; S1 is surface-level ruins), reducing the chance of confusion.
- **Shape backup:** Sporeglow appears on organic fungal forms (radial, circular, branching). Signal Yellow appears on manufactured signage (rectangular, gridded, symmetrical). Geometric distinction is absolute.

---

#### 4.6.2 Colorblind Mode: Enhanced Value Separation

As an accessibility option (not a replacement for the default palette), Rewild provides a **High Value Contrast** mode that increases the value gap between semantically distinct color groups:

| Color group | Normal mode value range | High Value Contrast adjustment |
| -- | -- | -- |
| Human-era (H1-H8) | 17-91 | Unchanged (already high range) |
| Biological (B1-B10) | 16-78 | Dark end (B1-B3) darkened by -5%. Bright end (B9-B10) brightened by +8%. |
| Transitional (T1-T4) | 35-55 | T1 darkened by -8%. T4 brightened by +8%. Widens the mid-range. |
| Synthetic (S1-S4) | 63-78 | Brightened by +10% across the board. Synthetic colors pop harder against all backgrounds. |

This mode does not change hue or saturation -- only value. The Decay Gradient's warm/cool story is preserved. The mode simply ensures that where hue distinctions fail, value distinctions compensate.

---

#### 4.6.3 Universal Backup Cues (Non-Color)

These are always active for all players. They ensure that no game-critical information depends solely on color:

| Information | Color cue | Shape/animation backup | Sound backup |
| -- | -- | -- | -- |
| Creature ecological role | Color temperature (warm = predator-adjacent, cool = prey-adjacent) | Silhouette taxonomy (Section 3.2). The primary signal. Shape alone is sufficient. | Predators: heavy, rhythmic ambient sound. Prey: light, scattered ambient sound. |
| Biome type | Dominant hue (green = reclaimed, teal = fungal, amber = thermal) | Dominant biological shape (branching = plant, radial = fungal, angular = thermal -- Section 3.3.4). | Biome-specific ambient soundscape. |
| Human vs. biological surface | Warm/muted vs. cool/saturated | Right angles vs. curves. Grid-aligned vs. ungridded. Uniform vs. variable stroke width. (Section 3.3). | Human surfaces: hollow acoustic quality (reverb, echo). Biological surfaces: dampened, absorbed sound. |
| Danger proximity | Palette desaturation (Tension), palette collapse (Pursuit) | Player creature animation shifts from relaxed to alert idle (Section 3.4.1). Screen parallax breath stops (Section 2.1). Shadow population begins (Section 2.2). | Ambient sound narrows. Low-frequency tension tone. Prey creature sounds cease. |
| Traversable surface | Higher value than surrounding surfaces | Flat-top contour (3+ horizontal pixels). The shape IS the signal. | Footfall sound on contact confirms traversability. |
| Shelter location | A6 (Shaftlight) warm glow from entrance | Shelter diagnostic shape: enclosed space with a single opening. The creature's directional indicator (Section 3.4.2) points toward it. | Interior shelter ambient: muffled exterior + enclosed warmth tone. |
| Evolution readiness | Bioluminescent pulse on creature body region | 2-frame pixel brightness cycle on a 4-second timer. The animation pattern is the signal, not the color. | Subtle bioacoustic pulse synced to the visual pulse. |
| Synthetic artifact presence | Synthetic palette colors (S1-S4) | Euclidean geometry, manufactured smoothness, grid alignment, bilateral symmetry. Human shapes in a biological world. | Material-specific sound on interaction: plastic resonance, glass ring, metal conductance. |

---

### 4.7 UI Palette

The Uninvited Frame principle (Section 1) mandates that UI color must not break immersion. The camera does not privilege the player, and the UI does not privilege the player's information needs over the world's visual integrity. UI elements use only colors already present in the current scene -- they borrow from the world rather than overlaying it.

---

#### 4.7.1 Diegetic UI Color Rules

Section 3.4 established that all UI information is communicated through the world itself (creature body state, environmental surfaces, organism behavior). This section defines the color constraints for the minimal UI elements that do exist.

**Rule 1: No unique UI color.** The UI palette is a strict subset of the master palette. No hex code exists that is used only by UI elements. If a color appears in the UI, it also appears in the world.

**Rule 2: UI elements inherit scene temperature.** Any UI-proximate element (the shelter direction indicator, the pause-state surface markings, the creature's body-state feedback) uses colors from the current biome's dominant ramp. In a fully reclaimed biome, the shelter indicator uses B4 (Chlorophyll). In a fresh ruin, it uses H4 (Mortar). The UI does not have its own color identity -- it takes the color of wherever the player is.

**Rule 3: UI legibility through value, not hue.** Where a UI element must be readable against a varied background, legibility is achieved by selecting a color at least 25 value points away from the average background value. Light backgrounds get dark UI marks. Dark backgrounds get light UI marks. The hue is borrowed from the scene. The value is adjusted for contrast.

---

#### 4.7.2 Specific UI Element Colors

| UI element | Color rule | Source |
| -- | -- | -- |
| **Shelter direction indicator** | Player creature's diagnostic ridge highlight color, at 60% opacity. Fades to 0% within 3 screens of shelter. | Creature palette (inherits from evolution state). |
| **Pause-state surface marks** | The darkest or lightest color in the current biome's dominant ramp (whichever provides more contrast against the surface the marks are rendered on). Rendered as etched/scratched texture matching the surface material. | Biome-dependent. H1 or H7 on human surfaces. B1 or B10 on biological surfaces. |
| **Health/damage feedback** | No separate UI color. Damage is shown through sprite degradation: edge pixels dim toward A1 (Void), body pixels flicker between their current color and -20% value. | A1, plus current creature palette at reduced value. |
| **Evolution readiness pulse** | Current biome's brightest biological color at +10% value, cycling over 4 seconds. | B5, B9, or H5-warm depending on biome (Section 4.5.5). |
| **Text (pause menu, if any)** | A6 (Shaftlight) on dark surfaces. A1 (Void) on light surfaces. Never both on the same screen. | Atmospheric ramp. |

---

#### 4.7.3 What the UI Never Uses

- No fully saturated colors. The UI never uses a color at higher saturation than the world around it. A UI element that is more vivid than the scene it overlays breaks the Uninvited Frame by drawing the eye to player-serving information.
- No synthetic artifact colors (S1-S4) in any UI context. These colors are reserved for narrative artifacts. Using Signal Yellow for a warning prompt or Screen Cyan for an interactive highlight would contaminate the chromatic archaeology system -- the player must never see a synthetic color and wonder "is that a UI element or a human artifact?"
- No fixed-position color blocks. No health bar, no stamina bar, no colored HUD frame. Any persistent colored rectangle in a fixed screen position reads as a game overlay, which violates the Uninvited Frame.
- No color coding for game mechanics. The game does not assign red=damage, green=health, blue=mana, or any other conventional color-to-mechanic mapping. The color system serves the world's ecology, not the player's convenience.

---

### 4.8 Color Review Checklist

Before any asset, scene, or palette variant enters the production pipeline, it must pass the following color-level review. This checklist is applied AFTER the shape review (Section 3.7) has been passed.

1. **Palette compliance test.** Every pixel in the asset uses a color from the 32-color master palette (or the current game-state variant of that palette). Open the asset in a palette-indexed editor and verify zero off-palette colors. If any pixel uses a color not in the palette file, it must be corrected to the nearest palette entry.

2. **Decay Gradient legibility test.** On any environment tile: can a reviewer determine the decay state (human-dominant, collision, or biologically-dominant) from the warm-to-cool ratio alone? If the tile reads as ambiguous, the ratio must be adjusted.

3. **Ramp span test.** No single tile uses more than 5 colors from any single ramp. If it does, the value range is too wide and implies a light source that the scene does not provide. Reduce to 4-5 ramp steps.

4. **Synthetic color containment test.** Do any synthetic artifact colors (S1-S4) appear outside of a Discovery/ruin context? If yes, they must be removed. These colors are quarantined from the biological world.

5. **Traversability value test.** On any gameplay screen: are traversable platforms rendered at higher value than non-traversable surfaces in the same tile column? If not, the value relationship must be corrected to maintain the bright=safe, dark=unsafe signal.

6. **Biome temperature test.** Does the tile's dominant color temperature match the biome archetype (Section 4.3)? Fresh ruin = warm-dominant. Fully reclaimed = cool-dominant. Fungal = teal-cool. Thermal = active-warm. If the temperature reads wrong, the color ratio must be adjusted.

7. **Colorblind safety test.** For any semantically distinct elements that share a hue range (per Section 4.6.1), does a non-color backup cue (shape, animation, value contrast) carry the distinction? Apply a protanopia simulation filter to the asset and verify that all gameplay-critical information remains legible.

8. **UI contamination test.** Does any UI element in the scene use a color that is not already present in the world layer of the same scene? If yes, the UI color must be replaced with a world-layer color at appropriate value contrast.

9. **Cardinal Rule color test.** For every color choice in the asset: can the artist explain why this surface is this color in ecological or physical terms? "This concrete is H3 because it is standard structural concrete in moderate shade." "This moss is B4 because it is actively photosynthesizing in indirect light." If the answer is "it looked good" or "it needed visual variety," the color is decorative, and the Cardinal Rule prohibits it.

---

## 5. Character Design Direction

### 5.0 Governing Principle

Every creature in Rewild is a solution to a problem the world posed. No creature exists for spectacle. No design choice is cosmetic. A predator's jaw shape tells you what it eats. A prey animal's coloring tells you where it hides. The player creature's dorsal ridge tells you what it has survived. Character design in this game is ecology made visible.

The cardinal rule applies with particular force here: **every pixel of every creature should look like nature solving a problem.** If you cannot articulate what survival pressure produced a visual feature, that feature does not belong in the design.

---

### 5.1 Player Creature Design

#### 5.1.1 Visual Archetype

The player creature is a **generalist mesofauna** — not apex, not base. Think the ecological niche of a raccoon, a crow, or a rat: an animal that survives not by being the strongest or fastest, but by being adaptable enough to exploit margins. It is visually unremarkable at first glance. It looks like something that *belongs* in this world, not something that was placed there for the player's benefit.

**Key read at a glance:** "Small thing. Moves carefully. Probably edible."

The creature should never look heroic, chosen, or special in its base state. It looks like one of dozens of small organisms navigating the ruin-ecology. What distinguishes it is not visual spectacle but **behavioral persistence** — the player makes it special through play, and the design must support that arc without front-loading it.

#### 5.1.2 Base Form Design Principles (16x16)

At 16x16, every pixel is load-bearing. The base form must be readable in a 3-4 color silhouette against any biome background in the game.

**Silhouette construction:**
- **Body mass:** Compact, slightly hunched. Center of gravity low. 8-10 pixels wide, 10-12 pixels tall in neutral standing pose. The proportions read as "cautious quadruped that can rear up" — think something between a salamander and a small mammal.
- **The dorsal ridge:** 2-3 pixels tall along the spine, breaking the top contour of the silhouette. This is the **single non-negotiable identifier.** At 16x16 it reads as a subtle crest or fin running from the back of the head to mid-spine. It is the visual through-line that persists across every evolutionary state. At base, it is small and uniform.
- **Limbs:** Four limbs suggested, not individually articulated at 16x16. At this scale, legs are 1-pixel contact points with the ground. Movement conveys limb count; standing pose conveys body plan.
- **Head:** Small relative to body. No visible eyes at 16x16 — the head is a shaped mass that implies sensory apparatus through contour, not through drawn features. Do not place a pixel-dot eye on a 16x16 creature. Let the head shape and behavior communicate awareness.
- **Tail or trailing element:** A 2-3 pixel trailing form that provides secondary motion in animation. This can evolve — it is mutable, unlike the ridge.

**Color at base state:**
- Body: 2-3 values from the biological ramp (mid-range, unsaturated). The creature should not pop against natural backgrounds.
- Dorsal ridge: 1 pixel width of a slightly more saturated or lighter value. Just enough to be trackable. This is a subtle luminance shift, not a color contrast.
- Underside/contact points: Darkest value used, grounding the form.

**What the player creature is NOT:**
- Not cute. Not ugly. Not distinctive. It is *ordinary* — an animal among animals.
- Not bilaterally decorated. No markings that read as "designed for the player to look at." Any patterning should read as camouflage, thermal regulation, or species signaling — functional patterning.
- Not anthropomorphized. No large eyes, no expressive brows, no proportional head-to-body ratio that suggests cartoon character.

#### 5.1.3 The Dorsal Ridge as Identity Anchor

The dorsal ridge is the **single persistent visual element** across all evolutionary states. It functions like a letterform — a shape the player learns to read unconsciously. Everything else about the creature may change. The ridge does not disappear; it *transforms.*

**Ridge evolution principles:**

| Evolution State | Ridge Read | Pixel Implementation |
|---|---|---|
| **Base (16x16)** | Low, uniform, subtle. A suggestion of a crest. | 2-3 pixels tall, same width as body contour, 1-color step above body value. |
| **Early adaptation** | Ridge responds to environment. Begins to speciate. | Height varies by 1 pixel along its length. Color may shift toward biome-appropriate hue. Gains a second color value. |
| **Mid evolution (24x24 range)** | Ridge becomes diagnostic. A biologist could classify this creature by its ridge alone. | Distinct silhouette profile — serrated, smooth, branching, or ridged depending on evolutionary path. 3-4 pixels tall at peak. May incorporate bioluminescent highlight pixel from transitional ramp. |
| **Late evolution (approaching 32x32)** | Ridge is prominent, complex, and unmistakably *this creature.* It is the visual record of everything the player survived. | Full sculptural element. 4-6 pixels tall. Multiple color values. May have secondary animation (flutter, pulse, raise/lower based on game state). The ridge is now the most visually complex part of the creature. |

**Critical rule:** The ridge must always break the top contour of the silhouette. In any pose, at any scale, from any direction, the dorsal ridge must be the element that makes the player creature's silhouette distinct from every other creature in the game. If you cover the ridge, the creature should be unidentifiable. If you show only the ridge, a player 10 hours in should recognize it.

**Ridge response to game state:**
- **Idle/safe:** Ridge rests flat or at minimal height.
- **Alert/sensing:** Ridge raises 1 pixel. Subtle, but the silhouette change is perceptible.
- **Threatened/combat:** Ridge fully erect. Maximum silhouette disruption. This is the creature's stress display.
- **Post-evolution:** Ridge pulses once with a transitional-ramp color on the frame the evolution completes. One pulse. Then it settles into its new resting form.

#### 5.1.4 Growth and Scale Progression

The creature grows from 16x16 to a maximum of 32x32. This is not a linear scale-up. It is a series of **resolution thresholds** where new visual information becomes possible.

**16x16 (Base):**
- Maximum abstraction. Every pixel is structural.
- 3-4 body colors. No internal detail. Silhouette and motion carry 100% of the read.
- Animation: 3-4 frame cycles. Movement is twitchy, quick, small-animal reactive.

**20x20 to 24x24 (Mid-evolution):**
- Enough resolution to distinguish head from body with a 1-pixel neck or contour change.
- Eyes become possible as single pixels — use sparingly. A single dark pixel in the head mass reads as "eye" only if the head shape supports it. Do not add eyes just because you have the pixels.
- Limbs gain individual articulation. Gait becomes readable.
- Dorsal ridge gains silhouette complexity.
- Animation: 4-6 frame cycles. Movement becomes more deliberate. The creature has gained mass and confidence.

**28x28 to 32x32 (Late evolution):**
- Full creature detail is possible. Internal markings, texture suggestion, distinct digits or appendage shapes.
- Secondary animation elements: breathing cycle, idle fidgets, environmental reactions (fur/scales responding to wind, etc.).
- The creature now has *presence.* It reads as a mid-tier organism, not a bottom-feeder. But it should never read as apex. Even at maximum evolution, the player creature should look like something that survives through cleverness, not dominance.
- Animation: 6-8 frame cycles. Weight and momentum are conveyed. The creature moves like something that has learned this world.

**Scaling rule:** When the creature grows, do not simply double the pixels. Each size threshold is a **redesign** that preserves the dorsal ridge profile and base body plan while adding detail appropriate to the new resolution. Think of it as the same animal photographed with increasingly better cameras, not a small animal inflated.

#### 5.1.5 Player Creature Animation Philosophy

**Principle: The creature animates like a documentary subject, not a game character.**

All player creature animation should feel like footage from a nature camera. The creature does not perform for the viewer. It acts on its environment.

**Locomotion:**
- Base walk: Quick, low, efficient. Weight stays close to the ground. At 16x16, this is a 3-frame cycle with visible contact-point shifting. The creature scurries.
- Run: Longer stride, body extends horizontally. The dorsal ridge flattens in motion (streamlining). 4-frame cycle at base.
- Climb: Limbs splay wide, body presses close to surface. Movement is halting — grip, shift, grip, shift. Convey that surfaces are being *negotiated,* not traversed.
- Swim (if applicable): Undulating body motion led by the tail. Dorsal ridge becomes a keel. Limbs tuck or paddle depending on evolutionary path.

**State communication through animation:**
- **Healthy, fed, rested:** Smooth motion cycles. Clean frame transitions. The creature moves efficiently.
- **Hungry/low resources:** Animation frames drop. Movement becomes jerky — frames 2 and 3 of a 4-frame walk merge or skip. The creature is conserving energy. This is a functional tell, not a cosmetic one.
- **Injured:** Asymmetric motion. If damaged on one side, that side's limb contact hesitates 1 frame. The creature favors the wound. At severe damage, the leading body contour drops 1 pixel (hunching, protecting core mass).
- **Evolving:** The creature pauses. 4-6 frame unique animation: body mass shifts, pixels reorganize. This is not flashy — it is biological. Think time-lapse of a chrysalis, not a transformation sequence. The dorsal ridge is the last element to settle into its new form.

**Idle animation:**
- The creature does small, animal things. Head turns. Weight shifts. Ridge settles and raises slightly. It investigates the ground pixel in front of it. These are 8-12 frame ambient cycles that make the creature feel *alive* without making it feel like it is waiting for the player to act.
- Idle animations should be **context-sensitive to the biome.** In organic environments: sniffing, foraging motions. In ruin environments: cautious scanning, stillness punctuated by quick head movements. The creature reads its environment even when the player is not directing it.

---

### 5.2 Creature Archetypes

The six-category shape taxonomy established in Section 3 defines how every non-player creature is built. This section expands each category into full character design direction: shape language, color usage, animation principles, and behavioral reads.

**Universal creature design rule:** No creature in Rewild has a face that communicates with the player. Creatures have sensory apparatus — eyes, antennae, heat pits, echolocation structures — that serve biological functions. They do not have expressions. Emotional read comes from **body shape, posture, and motion.**

#### 5.2.1 Apex Predators

**Shape language: Angular, convergent, massive.**

Apex predators are designed around **convergent lines** — shapes that point inward, toward a focal point, toward the prey. Their silhouettes taper. They are narrower at the point of contact (jaws, claws, striking appendage) than at the mass behind it. Think of them as **arrows made of muscle.**

**Design principles:**
- Silhouettes are dominated by acute angles. No soft curves on load-bearing structures.
- Mass is concentrated in the core/rear, tapering toward the head or primary attack vector. This reads as "all of this weight is behind the thing that hits you."
- Size: 48x48 to 64x64 minimum. Apex predators must dwarf the player creature at every evolution state. The size differential is the first and most important communication: *you are not this thing's equal.*
- Color: Drawn from the upper biological ramp — dark, saturated, dense values. Apex predators are not camouflaged; they do not need to hide. Their coloring says "I am visible because nothing here threatens me." 2-3 dominant dark values with a single high-contrast accent on the weapon structures (teeth, claws, stinger) drawn from the synthetic or transitional ramp.
- Internal detail: More than any other creature type, apex predators benefit from visible musculature, scarring, and textural variation. These are old, successful organisms. They look *used.*

**Animation principles — How apex predators MOVE:**

Apex predators move with **minimum necessary energy.** Every frame communicates efficiency and conserved power.

- **Idle/patrol:** Slow, even, metronomic. Long frames — hold each position 2-3 ticks longer than feels natural. This creates a sense of deliberateness. The creature is not reacting to the world; the world is reacting to it.
- **Alert/tracking:** The body stills. Only the sensory apparatus moves — head tracks, antennae orient, heat pits flare. The transition from patrol to alert should be a **freeze** followed by a single re-orientation. Predators do not fidget.
- **Pursuit:** Explosive. Frame rate doubles. The body elongates — the convergent silhouette stretches toward the target. All secondary motion (dangling elements, ambient body oscillation) locks into the direction of travel. The creature becomes a projectile.
- **Strike:** 2-3 frames maximum. The strike animation is faster than the player can consciously process. The player sees the wind-up (1 frame: body coils, mass shifts rearward) and the aftermath (body extended, weapon at full reach). The strike itself is 1 frame of blur or overlap. This communicates lethal speed.
- **Post-kill:** The creature returns to its slow, deliberate idle cycle. No celebration, no display. It fed or it defended. The emotional horror comes from the *return to calm.*

**Key behavioral animation:** Apex predators should have a visible **breathing cycle** — a 1-pixel expansion/contraction of the torso mass on a slow loop. This makes them feel alive and massive even when stationary. The player should feel the creature's metabolism.

#### 5.2.2 Ambush Predators

**Shape language: Deceptive. The resting shape lies.**

Ambush predators are the most complex character design challenge in the game. They must exist in two visual states — **disguise** and **reveal** — and the transition between them must recontextualize every pixel.

**Design principles — Disguise state:**
- The resting form mimics a specific environmental element: a rock formation, a plant cluster, a piece of ruin geometry, a harmless creature. The mimicry must be **good enough to fool the player on first encounter** but contain subtle tells that reward observation on subsequent encounters.
- Color: Drawn entirely from the palette ramp of whatever the creature mimics. If it mimics stone, it uses the decay/ruin palette. If it mimics flora, it uses the biological ramp. The creature's *own* coloring is only visible during the reveal.
- Shape: In disguise, the silhouette is indistinguishable from the mimicked object at game camera distance. The tells are in **micro-animation**: a breathing pixel, a too-regular pattern, a shadow that falls wrong.

**The tells — what distinguishes ambush predators from what they mimic:**
- **The symmetry break.** Natural objects and ruin geometry are irregular. Ambush predators in disguise are subtly *too symmetrical* — their mimicry is bilateral where the real object would be random. Train the player's eye to notice when a rock formation is a little too balanced.
- **The patience pixel.** One pixel on the creature, in disguise state, is on a very slow color cycle — shifting 1 palette step over 4-6 seconds, then shifting back. This is the creature's metabolism bleeding through the disguise. At normal play speed it is nearly invisible. A player who has learned to look for it can spot it.
- **The wrong shadow.** The creature's shadow, rendered using the Uninvited Frame principles, does not quite match the mimicked object's expected shadow. It is 1 pixel too wide, or it falls at a slightly wrong angle. The shadow tells the truth.

**Animation principles — The reveal:**
- The transition from disguise to attack is the single most violent animation beat in the creature's repertoire. It must feel like the *environment itself became hostile.*
- Frame 1: Disguise state (player sees a rock/plant/ruin piece).
- Frame 2: **The break.** The silhouette fractures. The mimicked shape splits along lines that were invisible in the disguise state. Angular, predatory geometry emerges from within the soft/inert disguise geometry. The creature's true color — drawn from the biological ramp, shifted toward high-saturation warning values — floods the form in a single frame.
- Frames 3-4: Full attack posture. The creature is now twice the visual size of its disguise state (folded anatomy unfurling). It is all angles, all weapon surfaces. The disguise elements either retract or become weapon components (a "rock" texture becomes armored plating; a "vine" becomes a striking tendril).
- Post-attack: If the ambush fails, the creature either pursues briefly (it is not built for sustained chase — convergent angles but lower mass than true apex predators) or resets to disguise state. The reset is *slow.* The player watches the horror fold itself back into something harmless. This is deeply unsettling by design.

**Scale:** Ambush predators range from 24x24 (mimicking small environmental features) to 48x48 (mimicking large ruin elements). Their disguise-state size should match real instances of the mimicked object in that biome.

#### 5.2.3 Prey Species

**Shape language: Dispersed, soft, peripheral.**

Prey species are designed around **divergent shapes** — forms that spread outward, that distribute mass rather than concentrating it. Their silhouettes are wide, low, and round. Where predators converge toward a weapon point, prey species **diffuse** toward escape vectors.

**Design principles:**
- Silhouettes are dominated by curves and obtuse angles. No sharp protrusions, no tapering toward contact points.
- Mass is distributed evenly or concentrated in the center (protective core) with dispersed peripheral elements (wide-set limbs, fanned sensory arrays, spread appendages). This reads as "I am trying to be everywhere at once" — vigilance made structural.
- Size: 8x8 to 24x24. Many prey species are smaller than the player creature. They exist in numbers, not as individuals.
- Color: Mid-to-low biological ramp values. Desaturated, environment-matched. Prey species are the most camouflaged creatures in the game. Their palettes should be indistinguishable from the biome's ground cover and foliage at game camera distance. When a prey creature moves, it should seem to *materialize from the background.*
- Group identity: Prey species should be designed with **variation templates** — 2-3 pixel-level variations of the same base sprite that can be deployed in groups. No two individuals are identical, but all are clearly the same species. This is the visual shorthand for "population," not "character."

**How prey species signal "harmless":**
- **Rounded top contour.** The highest point of the silhouette is a smooth arc, never a point or ridge. This is the opposite of the predator's angular crown.
- **Visible, exaggerated sensory structures.** Large ear-analogs, spread antennae, wide-set eye-positions. These read as "I am built to detect danger, not to cause it." The creature's visual budget is spent on input, not output.
- **Contact points below center of mass.** Limbs and ground-contact elements are positioned under or behind the body center. This reads as "ready to flee" — the body is already ahead of the feet.
- **No weapon surfaces.** No claws, no teeth, no spines. If a prey species has defensive structures (shell, camouflage, toxic coloring), these are **passive** — they do not protrude or threaten. They protect. The visual distinction is critical: a spine that curves inward (defensive) vs. a spine that points outward (offensive).

**Animation principles:**
- **Primary motion: the startle.** Prey species have a 1-frame full-body flinch animation that triggers on any sudden environmental change (player movement, predator detection, loud event). The body compresses 1 pixel vertically and expands 1 pixel horizontally — a shock-absorber response. This flinch should be *fast and universal* across all prey species. It is the taxonomic tell.
- **Locomotion: burst-and-freeze.** Prey species do not move at constant speed. They move in rapid bursts (3-4 frames of fast displacement) followed by freeze frames (2-3 frames of absolute stillness). This mimics real prey animal movement — sprint to new cover, become invisible, sprint again.
- **Group animation:** When multiple prey creatures are on screen, their burst-freeze cycles should be **slightly desynchronized** — not perfectly offset, but organically staggered. This creates the ripple effect of a startled herd without requiring coordinated group AI animation.
- **Feeding/foraging:** Low, ground-focused, continuous. Small repetitive motions — pecking, grazing, rooting. These animations are visually *boring* by design. They communicate "this creature's life is routine and small." The contrast with the predator's deliberate stillness is important: prey are busy being alive; predators are busy waiting.

#### 5.2.4 Scavengers

**Shape language: Asymmetric, low, opportunistic geometry.**

Scavengers are the most visually *irregular* creatures in the game. Where predators are angular and prey are round, scavengers are **lopsided.** They look jury-rigged, cobbled together, as if evolution built them from whatever was available.

**Design principles:**
- Silhouettes are asymmetric. One side of the body is visually heavier than the other. One limb is longer, one sensory structure is more prominent, the body leans. This asymmetry reads as "not optimized for any one thing" — the visual signature of a generalist.
- Mass is concentrated low and forward. Scavengers are built to reach downward and into — into carcasses, into gaps, into places other creatures cannot or will not go. Their body plan says "I can get to what you left behind."
- Size: 16x16 to 32x32. Roughly player-scale. Scavengers occupy the same ecological tier as the player creature, which makes them both competitors and barometers — the player should learn to watch scavenger behavior for environmental information.
- Color: The widest palette range of any creature type. Scavengers pull from biological, transitional, AND decay ramps. They look like they have been everywhere. Staining, discoloration, and palette-mixing are appropriate. A scavenger might have biological coloring on its core body with ruin-palette staining on its lower limbs (from rooting in decay zones). This is the visual story of their diet.

**How scavengers read as "opportunistic":**
- **The lean.** Scavengers are always visually leaning toward something — their idle pose is not neutral. They tilt toward food sources, toward the player, toward exits. Their center of gravity is displaced from their center of mass. They look like they are about to *go somewhere.*
- **Tool surfaces, not weapon surfaces.** Scavengers may have structures that resemble predator weapons (strong mandibles, gripping limbs) but these are visually coded as **tools** — blunter, broader, multi-purpose. A predator's claw tapers to a point; a scavenger's gripping appendage is wide and flat, built for leverage, not piercing.
- **Worn appearance.** More pixel-level texture variation than any other creature type. Scavengers look weathered, patchy, unevenly colored. They are surviving, not thriving. This is communicated through broken pattern regularity — where a prey species has smooth, consistent coloring, a scavenger has irregular value shifts across its body.

**Animation principles:**
- **Primary motion: the pivot.** Scavengers constantly reorient. Their idle animation is a slow rotation of the head/sensory mass, scanning 180 degrees on a 6-8 frame loop. They are always assessing. Every 2-3 scan cycles, the entire body pivots to face a new direction. The creature looks *restless.*
- **Locomotion: the hustle.** Scavengers move at medium speed with irregular rhythm. Not the prey's burst-freeze, not the predator's metronomic patrol. Their walk cycle has a hitch — a frame that is slightly longer than the others, creating a loping, uneven gait. They move like they are always slightly rushing.
- **Approach animation:** When moving toward a food source or point of interest, scavengers exhibit a distinctive **stop-start-commit** pattern: approach 4-5 pixels, stop, lower body (compression pose), then rush the remaining distance. This telegraphs opportunism — caution followed by sudden greed.
- **Retreat:** Fast, undignified, no commitment. Scavengers flee at the first sign of credible threat, and their flee animation is deliberately less graceful than their approach. They trip over their own asymmetry. This is not played for comedy — it communicates "this creature is not built for confrontation."

#### 5.2.5 Territorial Defenders

**Shape language: Radial, bounded, symmetric.**

Territorial defenders are designed around **boundaries.** Their visual language communicates perimeter — where you may go and where you may not. They are the most geometrically regular non-player creatures in the game.

**Design principles:**
- Silhouettes emphasize bilateral or radial symmetry. Defenders are visually *stable* — wide base, centered mass, even weight distribution. They look rooted. Where prey species are ready to flee and predators are ready to pursue, defenders look like they have *already arrived.*
- Mass is distributed with emphasis on a defined front face — a broad, shielded, display surface that faces outward from the territory. Think armored plates, extended crests, widened body profiles. The creature is a wall.
- Size: Variable, 24x24 to 48x48, but always proportioned to the territory they defend. A small-territory defender guarding a single resource node might be 24x24. A perimeter defender controlling a passage is 48x48 and positioned to fill the space.
- Color: **High-contrast warning palettes.** Territorial defenders are the most visually conspicuous creatures after apex predators. They use strong value contrasts — dark body with bright boundary markings, or saturated patches on desaturated base colors. Drawn from the biological ramp at its most saturated, often incorporating 1-2 transitional-ramp highlight colors. They are meant to be seen. Their coloring says "you have been warned."

**How territorial defenders signal "boundary":**
- **The threshold display.** Defenders have a specific animation state that activates when the player approaches territory boundaries. The body broadens — appendages spread, crests raise, display surfaces orient toward the intruder. The creature *expands* to fill visual space. This is the first warning.
- **Facing behavior.** Territorial defenders always orient their display face toward the nearest potential intruder. Their sprite rotation is locked to the player's position relative to the territory center. This tracking behavior is itself a visual signal — the creature watches you *before* you knew it was watching.
- **Boundary markers.** Some territorial defenders produce environmental visual elements — pixel-scale markers (scat, scratches, scent-indicators rendered as small colored pixels on terrain surfaces) at the edges of their territory. These are not the creatures themselves but design elements that extend the creature's visual presence beyond its sprite bounds. The creature's color palette should be echoed in these markers.
- **Escalation postures.** Three distinct visual states communicate the warning system:
  - **Aware (player at territory edge):** Orientation only. Body turns to face player. No shape change.
  - **Warning (player crossing boundary):** Display expansion. Body mass increases by 20-30% through raised crests, spread limbs, deployed armor. Color shifts — warning-palette elements brighten by 1 palette step.
  - **Aggression (player within territory):** Full threat posture. Maximum visual size. All display elements fully deployed. The creature begins approach. Animation frame rate increases. The warning period is over.

**Animation principles:**
- **Patrol: the circuit.** Defenders move in visible, repeatable patterns along territory boundaries. Their patrol path should be readable by an observant player within 2-3 cycles. This is a design feature — mastery means learning the patrol, finding the gap.
- **Station-keeping.** When not actively patrolling, defenders hold position with minimal movement. Their idle animation is subtle — weight shifts, sensory scanning — but they do not wander. They *occupy space.*
- **Combat:** Defenders do not pursue beyond territory boundaries. Their attack animation is a short-range, high-power lunge or strike — the visual language of "I will hurt you here but not chase you there." If the player retreats past the boundary, the defender executes a **return-and-display** animation: backs up to optimal position, performs one final expansion display, then resets to patrol.

#### 5.2.6 Semi-Intelligent Creatures

**Shape language: Irregular but intentional. Evidence of problem-solving made structural.**

Semi-intelligent creatures are the most unsettling category because they break the rules that govern every other archetype. Their visual design communicates **agency** — not human-level intelligence, but the kind of adaptive problem-solving that makes the player pause and think "that was deliberate."

**Design principles:**
- Silhouettes combine elements from other archetypes in non-standard ways. A creature with a predator's angular head on a scavenger's asymmetric body. A prey-species frame with a defender's display structures used for a completely different purpose. The visual language says "this creature is not cleanly categorized."
- **Tool use is visible in anatomy.** Semi-intelligent creatures have appendages or structures that read as multi-purpose — limbs that grip AND walk, sensory structures that probe AND manipulate. Where other creatures have anatomy specialized for one function, semi-intelligent creatures have anatomy that suggests *choice.*
- Size: 24x24 to 40x40. Deliberately overlapping with the player creature's evolution range. This creates visual kinship — an uncanny recognition that these creatures occupy a similar cognitive tier.
- Color: Muted biological ramp with **one anomalous color element** drawn from the synthetic or transitional palette. This single synthetic-ramp element is the visual marker of the semi-intelligent category. It reads as "something about this creature's relationship with the post-human environment is different." It might be a colored marking that mimics a ruin-pattern, or an eye-analog that reflects a transitional-palette bioluminescent color. One element. Not more. Subtlety is the entire point.

**What visual language communicates "semi-intelligent":**
- **Object interaction animation.** Semi-intelligent creatures have unique animations for interacting with environmental objects that no other archetype possesses. They push things. They arrange things. They carry things. These animations are slow and deliberate — 8-12 frames for a single interaction — and should read as *considered action,* not instinct.
- **The pause.** Before any significant action, semi-intelligent creatures execute a 4-6 frame pause where the body stills and the head orients toward the target of the action. This is the visual language of planning. No other creature type does this. Predators stalk; they do not plan. Prey react; they do not consider. The pause is the behavioral signature.
- **Social spacing.** When multiple semi-intelligent creatures are present, they maintain specific distances from each other — not the random dispersal of prey herds or the overlap-tolerance of scavenger groups. They space themselves with *intention.* This is communicated through subtle position adjustments: if one creature moves, nearby individuals shift to maintain consistent distances.
- **Environmental modification.** Semi-intelligent creatures alter their surroundings in small, visible ways — moved objects, arranged debris, cleared paths. The player should be able to identify semi-intelligent creature territory not by the creatures themselves but by the evidence of *organized behavior* in the environment. Pixel art implementation: small 4x4 to 8x8 object clusters that use the same placement logic as the creatures' boundary markers but arranged in non-random patterns (rows, circles, stacks).

**Animation principles:**
- **Movement: deliberate and variable.** Semi-intelligent creatures do not have a single walk cycle. They have context-dependent locomotion — moving slowly and carefully in unfamiliar areas, moving quickly and confidently in known territory, stopping at obstacles and visibly *solving* them (finding a route around, testing a surface). Their animation library is the largest of any non-player creature.
- **Idle: purposeful.** Semi-intelligent creatures do not have true idle animations. When not traveling, they are engaged in visible tasks — manipulating objects, grooming, inspecting their environment, observing other creatures. "Standing still and doing nothing" should be rare enough that when it occurs, it reads as a meaningful choice (resting, watching).
- **Response to player:** Unlike other archetypes, semi-intelligent creatures have a distinct **observation** response to the player that is neither flight, aggression, nor indifference. They watch. The head tracks, the body orients, but no threat-response or escape-response activates. This neutral observation should be deeply unnerving for the player. Something is looking at them and *thinking about them.*

---

### 5.3 Devolved Humans

#### 5.3.1 Design Philosophy

This is the most emotionally dangerous character design in the game. Get it wrong and it is either comical or grotesque. Get it right and it is a quiet devastation — the moment the player realizes what they are looking at, and then the longer moment when they realize these creatures do not realize what they used to be.

**The governing design question:** How do you make a creature that is recognizably post-human without making it human?

**The answer:** You design an animal that has *human residue.* Not a human that has become animal-like — that reads as degradation, as tragedy performed. Instead: an animal that happens to share structural ancestry with humans. The difference is in where the emotional weight sits. A degraded human asks the player to mourn. A post-human animal asks the player to accept.

**The design must never:**
- Be played for horror. These are not zombies, mutants, or monsters. They are animals with a specific evolutionary history.
- Be played for pathos through posed vulnerability. No reaching hands, no almost-human expressions, no visual pleas for recognition.
- Be anatomically human. No visible human faces. No human hands. No upright posture maintained as a vestigial trait.
- Be individually distinctive in human-readable ways. They do not have "characters." They have phenotypes.

**The design must always:**
- Be fully ecologically plausible. These creatures occupy a real niche. They eat specific things, avoid specific threats, and have body plans adapted to their actual lifestyle.
- Reward slow observation. The human ancestry should be something the player notices over time, not on first sight.
- Generate emotion through *recognition,* not through *performance.* The creature is not sad. The player is sad. The creature is just living.

#### 5.3.2 Physical Design

**Body plan:**
- Quadrupedal or semi-quadrupedal. The spine has reoriented — these creatures have been non-upright for thousands of generations. The spine curves horizontally. The pelvis has adapted. They do not stand up, and nothing about their posture suggests they once did.
- Forelimbs and hindlimbs are similar in length and function — convergent with other quadrupedal mesofauna. The forelimbs are NOT recognizable as arms. They are legs. However — and this is the critical detail — the forelimbs retain a slightly wider range of articulation than the hindlimbs. Not enough to grasp tools. Enough that a biologist would note "unusual forelimb flexibility in this species."
- **The hands are the tell.** Devolved humans have feet on all four limbs, but the front feet have one too many toes. Five digits, slightly longer than functional quadrupedal anatomy requires, with the innermost digit set at a slightly different angle. At 32x32 sprite scale, this reads as a 1-pixel anomaly in the foot shape — a front foot that is just slightly more complex than it needs to be. The player who notices this is the player who understands.
- Head: Rounded, larger relative to body than most quadrupeds of similar size. Housed forward on a shortened neck. No visible face at game camera distance. The cranium is still large — evolution did not remove the brain case, but the contents have reorganized. At close-up scale (cutscene or lore-illustration context only), the face is flat, the nose is minimal, the jaw is prognathic, and the eyes are wide-set and animal-blank. **At game camera distance, the head is a rounded mass. Do not render human facial features at gameplay scale.** The head reads as "large-brained animal."
- Skin/covering: Sparse, coarse hair over pale, mottled skin. Not fur — not dense enough. The skin is visible between patches. Use 2-3 biological-ramp values with low saturation, leaning toward the pallid end of the spectrum. These creatures read as *naked* in the way that some mammals read as naked — not exposed, just lightly covered. The hair pattern is irregular, concentrated on the spine and limbs, thin on the flanks.
- Size: 28x28 to 36x36. Close to the player creature's upper evolution range. This size proximity is uncomfortable by design.

#### 5.3.3 Behavioral Animation — The Herding Read

Devolved humans are herding animals. Their animation design centers on **group behavior** — they are never encountered alone and should never be animated as individuals with group AI bolted on. They are a herd that contains individuals, not individuals that form a herd.

**Herd behavior animation:**
- **Grazing/foraging:** Low, repetitive, ground-focused. The head bobs — down (feeding), up (scanning), down (feeding). The up-scan is quick (2 frames) and the down-feed is long (6-8 frames). This is the baseline animation and should be happening for 80%+ of screen time.
- **Herd movement:** The group moves as a loose blob. No formation, no leader. Direction changes propagate through the group like a slow wave — one individual turns, neighbors follow 4-8 frames later, and the shift ripples through. This is ungulate herd dynamics, not human social organization.
- **Contact comfort:** Individuals in the herd maintain close physical proximity. When stationary, they cluster. Pixel overlap is acceptable and intentional — bodies touching, limbs sharing space. This is animal contact-comfort behavior (think primates grooming, cattle standing flank-to-flank), not human social behavior. **Do not animate interactions that read as conversation, cooperation, or coordinated task-completion.** These creatures do not talk to each other. They warm each other.
- **Alarm response:** When startled, the herd executes a prey-species burst-and-freeze, but slower and less coordinated than purpose-built prey. They startle *badly* — bumping into each other, reversing direction, freezing at slightly different times. Their alarm response is less efficient than true prey species because they are not under the same selection pressure. They are mid-tier enough to survive some predation through herd size rather than individual escape ability.
- **The moment that breaks the player:** Devolved humans have ONE animation behavior that is vestigial and serves no survival function. It is not language. It is not tool use. It is not art. It is this: **When settling to rest, an individual occasionally extends one forelimb and places it on the flank of a nearby individual, holds for 6-8 frames, then withdraws it.** This is a comfort behavior — purposeless, non-survival-oriented, probably a residual grooming initiation reflex. It is the most human thing they do, and it means nothing to them. Animate it rarely. 10-15% chance per rest cycle. Let the player find it.

#### 5.3.4 Integration with the Ecology

Devolved humans are prey-to-mid-tier organisms. They are hunted. They are parasitized. They compete for resources with other mid-tier organisms including, potentially, the player creature.

**Visual design must support these ecological interactions:**
- They exhibit alarm coloring shifts — pale skin flushes 1 palette step toward a warmer value under stress (increased blood flow to skin surface). This is visible at gameplay distance and serves as a status indicator for the observant player.
- They leave environmental traces consistent with their body plan — worn paths through vegetation, gathered sleeping-area depressions, scattered food remnants. These traces look like animal traces, not human habitation.
- Predator interactions: When taken by a predator, devolved humans do not scream in recognizably human ways. They make *animal distress* — communicated through body animation (thrashing, compression, struggle-and-go-limp cycle), not through sound design cues that read as human. The visual treatment of their death must be consistent with how any prey animal's death is depicted. **Equal dignity. They are animals and they die as animals die.**

---

### 5.4 Expression and Pose Style

#### 5.4.1 The Anti-Expression Mandate

**Creatures in Rewild do not emote. They behave.**

This is not a limitation of the pixel art style. This is a design philosophy. The temptation in character animation — especially at low resolution — is to give creatures readable emotional states through simplified facial expressions: dot-eyes that widen for fear, mouth-pixels that curve for aggression, body language that poses toward the camera. Rewild does none of this.

**Why:** Because the game's pillar is "The World Doesn't Care About You," and creatures that perform emotions for the player's benefit violate that pillar at the foundational level. An animal that makes a "scared face" is communicating with the player. An animal that freezes, compresses its body, and orients its sensory apparatus toward a threat is communicating with its environment. The player reads the emotion, but the emotion is not *for* them.

#### 5.4.2 What Replaces Expression

**Body state replaces facial expression.** The full-body silhouette communicates what a face would in a conventional game:

| Emotional Read | Body-State Implementation |
|---|---|
| **Fear** | Body compresses vertically. Limbs tuck. Silhouette shrinks. Center of mass lowers. The creature is making itself small. |
| **Aggression** | Body expands. Appendages deploy. Silhouette grows. Center of mass rises or shifts forward. The creature is making itself large. |
| **Curiosity/attention** | Head extends forward. Body elongates horizontally toward the object of interest. Weight shifts to leading limbs. One part of the body reaches while the rest holds back. |
| **Pain** | Asymmetric compression. The body folds around the injury site. Locomotion becomes uneven. The silhouette loses its species-typical symmetry. |
| **Rest/safety** | Body relaxes to widest, lowest posture. Limbs extend. Silhouette becomes horizontal and ground-hugging. Breathing cycle (1-pixel expansion/contraction) becomes the dominant animation. |
| **Alertness** | Full stillness. All motion ceases except sensory-structure orientation. The silhouette locks. In a world of ambient animation, the sudden *absence* of movement reads as hyperawareness. |

#### 5.4.3 Personality Through Behavioral Repertoire

Creatures express personality through the **breadth and specificity of their behavioral library,** not through characterization. A creature feels like an individual when it has enough distinct behaviors that the player starts to perceive a pattern. This is emergent personality, not designed personality.

**Implementation guidelines:**
- **Minimum behavioral library per creature:** 4 distinct animation states (idle, locomotion, threat-response, feeding/interaction). This is enough for background fauna that the player does not study closely.
- **Standard behavioral library:** 6-8 animation states. Add context-sensitive idles (different idle in different biomes or weather), social behaviors (if a herding/social species), and environmental interaction (investigating objects, avoiding hazards).
- **Deep behavioral library (key species, semi-intelligent creatures, devolved humans):** 10-14 animation states. Full context-sensitive behavior sets. Rare/conditional animations that play infrequently. State-transition animations between every major state (not just pop-cuts between idle and walk, but visible acceleration, deceleration, and reorientation frames).

**The personality rule:** If a player watches a creature for 60 seconds and describes what it "is like" — "cautious," "lazy," "nervous," "bold" — the animation design has succeeded. If a player watches for 60 seconds and describes what it "is feeling" — "happy," "sad," "angry" — the animation design has failed. Behavior, not emotion. Pattern, not performance.

#### 5.4.4 Pose Language at Pixel Scale

At 16x16 to 32x32, pose is the primary expressive tool. Every keyframe of every animation is a **pose** that must read as a complete silhouette statement.

**Pose principles:**
- **One major axis shift per pose.** A pose changes one major thing from the previous pose — body angle, limb configuration, head orientation, center of gravity. Not two. Not three. At pixel scale, each change must read independently before the next change occurs.
- **Line of action is literal.** At this resolution, the creature's spine (or primary body axis) is a readable line of 4-8 pixels. That line's shape IS the pose. A curved line reads as relaxed or recoiling. A straight line reads as alert or aggressive. An S-curve reads as in-motion. This line should be the first thing drawn in every keyframe.
- **Ground contact tells the story.** How many pixels touch the ground surface, and where they are relative to the body center, communicates stance instantly. All pixels grounded = stable/resting. Minimal pixel contact = mobile/tense. No ground contact = airborne/falling/leaping. The ground relationship is more readable than any limb position at this scale.
- **Anticipation and follow-through in 1-pixel increments.** Before a creature moves, its center of mass shifts 1 pixel in the movement direction (anticipation). After stopping, the leading contour continues 1 pixel and returns (follow-through). These 1-pixel movements are nearly subliminal but their absence makes animation feel robotic. They are mandatory for all creatures 20x20 and above.

---

### 5.5 Scale and Readability

#### 5.5.1 The Scale Problem

Rewild contains creatures ranging from 8x8 (small prey) to 64x64+ (apex predators). These creatures must coexist on the same screen, remain individually readable, and maintain the game's ecological realism. An 8x8 creature and a 64x64 creature in the same frame is a 64:1 pixel-area ratio. This requires deliberate LOD strategy.

#### 5.5.2 LOD Philosophy

Level of detail in Rewild is not about polygon reduction or texture resolution. It is about **information priority at gameplay distance.** Every creature has a hierarchy of visual information:

**Tier 1 — The silhouette read (always visible):**
What archetype is this? (Predator angular, prey round, defender broad, scavenger asymmetric.) What size class? What facing direction? This information must read at any camera distance, at any creature scale, against any background.

Implementation: The outermost pixel contour of every creature is the most important design element. It must use maximum-contrast values against the creature's own body color. The silhouette outline is where the shape taxonomy lives. If you strip away all interior pixels, the outline alone should communicate archetype.

**Tier 2 — The state read (visible at gameplay distance):**
What is this creature doing? Moving, idle, alert, feeding, threatening? This information must read at standard gameplay camera distance for creatures 16x16 and above.

Implementation: Body-state posing (Section 5.4). The overall shape and proportional relationships change to communicate state. This tier relies on the silhouette shift, not on internal detail.

**Tier 3 — The identity read (visible on attention):**
What species is this, specifically? What evolutionary state is the player creature in? Is this the same individual I saw earlier? This information is for engaged observation, not at-a-glance recognition.

Implementation: Internal color patterning, specific anatomical details, dorsal ridge state (for the player creature). This detail exists but is not relied upon for gameplay-critical reads.

**Tier 4 — The intimacy read (visible on close study):**
Is this creature injured? Stressed? What has it been doing? What is its relationship to this specific environment? This is reward-level detail for the player who stops and watches.

Implementation: Subtle animation details (the patience pixel, the stress flush, the comfort behavior), environmental staining, damage asymmetry. This tier may only be perceptible to players actively looking for it.

#### 5.5.3 Small Creatures at Game Scale (8x8 to 16x16)

Creatures at this scale are **shapes that move.** They communicate through silhouette and behavior only.

- Maximum 3-4 body colors. No internal patterning. Color serves silhouette reinforcement (lighter top/darker bottom, or high-saturation mark against neutral body).
- Animation frames: 2-4 per cycle. Convey movement type (scurrying, hopping, slithering, flying) through the minimum number of distinct poses.
- Readability test: If the creature is 1/8th the height of the screen and moving against a complex background, can you tell its archetype in under 1 second? If not, simplify.
- Group rendering: At this scale, creatures are often deployed in groups. Individual detail matters less than **group behavior read.** A cluster of 8x8 creatures should read as "swarm," "flock," "scatter," or "herd" based on their collective motion pattern, not their individual designs.

#### 5.5.4 Medium Creatures at Game Scale (20x20 to 36x36)

The player creature and most ecologically significant species live at this scale. This is where the highest design resolution investment should be focused.

- 4-8 body colors. Internal patterning is possible and should be used for species identification and environmental storytelling (see Decay Gradient application for creatures that traverse biome boundaries).
- Animation frames: 4-8 per cycle for primary actions. Budget secondary/rare animations at 6-12 frames.
- Readability test: At standard gameplay camera, can you distinguish this creature's species from other same-archetype creatures within 2-3 seconds of observation? If not, the species-distinguishing features need amplification.

#### 5.5.5 Large Creatures at Game Scale (40x40 to 64x64+)

Apex predators and large territorial defenders. These creatures are visual events — their presence on screen reshapes the composition.

- 6-12 body colors. Full internal detail: texture, musculature, scarring, environmental wear. At this scale, the creature is a set piece.
- Animation frames: 6-10 per primary action cycle. Large creatures can afford slower animation because their size provides visual impact even at low frame counts. Slower animation on large creatures reads as weight and power. Do not make large creatures animate at the frame rate of small creatures — it makes them look frantic rather than massive.
- **Screen presence rule:** A 64x64 creature at standard camera distance occupies a significant portion of the visible frame. Its design must account for this — the creature needs to look correct as both a standalone sprite AND as a compositional element of the full screen. Its coloring must interact well with the biome palette behind it. Its animation must not overwhelm the motion hierarchy of the scene (the player creature's motion must still be the most trackable element).
- **Entrance and exit animations:** Large creatures should not simply walk on and off screen at the sprite edges. Their approach should be foreshadowed — shadow, sound-response from nearby small creatures, environmental disturbance — before the creature itself is visible. Their departure should linger — trailing elements, environmental settling. The screen should *remember* that a large creature was there.

#### 5.5.6 Multi-Scale Encounter Composition

When creatures of vastly different sizes share the screen:

- **The player creature is always mid-ground in the visual stack.** Small creatures render in front of or behind the player. Large creatures render behind or create a visual frame around the player. The player creature should never be visually lost behind a larger creature during gameplay.
- **Motion hierarchy:** Player creature > creature the player is currently interacting with > large ambient creatures > small ambient creatures. This is implemented through animation timing — higher-priority creatures animate at full frame rate; lower-priority creatures can drop to 1/2 or 1/3 frame rate when not the focus of interaction. This is both a readability strategy and a performance strategy.
- **Contrast anchoring:** In multi-scale scenes, the player creature should always have the highest local contrast against its immediate background. If a large creature's body creates the background behind the player creature, the large creature's palette in that zone must provide sufficient contrast for the player to read clearly against it. This may require per-encounter palette adjustments for large creature zones that overlap with player space.

---

### 5.6 Summary Production Checklist

For every creature sprite sheet delivered, artists must verify:

1. **Silhouette test:** Does the creature read as its archetype at 50% zoom? Cover the interior — does the outline alone communicate angular/round/broad/asymmetric/irregular?
2. **State test:** Can you distinguish idle, locomotion, and threat-response from silhouette change alone?
3. **Scale test:** When placed alongside the player creature sprite at the same zoom level, does the size relationship immediately communicate the ecological power dynamic?
4. **Palette compliance:** Are all colors drawn from the 32-color master palette? Do they conform to the palette ramp assignments for the creature's archetype?
5. **Animation minimum:** Does the creature have at minimum 4 animation states (idle, move, respond-to-threat, interact)?
6. **The Cardinal Rule test:** Can you articulate what survival pressure or ecological function produced every visible feature? If any element exists for visual appeal alone, it fails. Redesign it to communicate function, or remove it.
7. **Player creature dorsal ridge check:** (Player creature only.) Is the dorsal ridge visible, contour-breaking, and consistent with the current evolution state in every single frame of every single animation?

---

## 6. Environment Design Language

### 6.1 Architectural Style and History

#### The Civilization That Was

The human civilization in Rewild was not a single aesthetic. It was layered, pragmatic, and ultimately hubristic. The architecture tells the story of a society that kept building on top of itself rather than reckoning with what it had already built. Three distinct architectural eras are visible in the ruins, and an environment artist should be able to identify which era a structure belongs to by its geometry alone.

**Era 1 — Foundational Industrial (oldest, deepest strata)**
- Heavy rectangular forms, thick walls (3–4 tile widths at 16x16)
- Exposed structural elements: I-beams, riveted plates, concrete block
- Grid-aligned with absolute regularity — no curves, no ornamentation
- Function: factories, utilities, infrastructure, sewers, transit tunnels
- Visual shorthand: if it is underground and brutally geometric, it is Era 1
- Pixel note: use the darkest values from the warm/muted end of the Decay Gradient; these surfaces should feel like they have absorbed decades of grime

**Era 2 — Corporate/Institutional (middle strata, most common)**
- Cleaner lines than Era 1 but still Euclidean
- Glass-and-steel aesthetic: thinner walls (1–2 tiles), large window openings, drop ceilings
- Modular, repetitive floor plans — hallways of identical rooms, cubicle grids, atria
- Function: offices, hospitals, schools, retail, residential towers
- Visual shorthand: if it has drywall, fluorescent light fixtures (dead), and standardized door frames, it is Era 2
- These are the ruins the player encounters most. They should feel *familiar* — mundane spaces made eerie by emptiness and overgrowth

**Era 3 — Late-Stage Technological (newest, surface/upper strata)**
- Curved and angular geometries that attempted to break from the grid but were still fundamentally built on Era 2 foundations
- Smart-glass panels (now opaque/shattered), solar cladding (now cracked), vertical garden infrastructure (now wildly overgrown)
- Function: research campuses, data centers, automated facilities, luxury residential
- Visual shorthand: if it has curves sitting on top of straight lines, or technological surfaces that have become substrates for growth, it is Era 3
- These spaces carry the most irony. The "green" buildings are the ones nature has most aggressively consumed, because they were already designed with biological interfaces

#### Architectural Storytelling Rules

1. **Strata are literal.** Deeper in the map = older construction. The player descends through architectural history. An artist tiling a deep room should reach for Era 1 assets; a surface room should show Era 3.

2. **No building is pure.** Most structures show at least two eras. An Era 2 office built on an Era 1 foundation, with an Era 3 solar retrofit on the roof. Seams between eras should be visible — different tile materials meeting at junction lines.

3. **Geometry encodes function.** The player should be able to look at a room's shape and guess what it was before reading any props. Classrooms are wider than deep, with one flat wall. Server rooms are narrow and long. Stairwells are vertical shafts. Kitchens have counter-height horizontal surfaces. Maintain these proportional signatures.

4. **Collapse follows structure.** When architecture breaks down, it breaks along its own logic. Concrete cracks in blocks. Steel bends but holds. Glass is simply gone. Wood rots from the inside. Drywall dissolves. An artist should ask: "What would fail first in this room?" and remove or damage those elements before the structural ones.

---

### 6.2 Texture Philosophy

#### The Pixel Art Surface Contract

At 16x16 tile resolution (our primary tile size, with 8x8 used for detail tiles and small props), every pixel is a commitment. There is no room for noise that does not communicate material.

#### Color Budget Per Tile

- **Hard surfaces (concrete, metal, glass):** 3–4 colors maximum per tile. One base value, one shadow, one highlight, one accent (rust, stain, crack). Concrete tiles that use 5+ colors will read as busy and lose their material identity.
- **Organic surfaces (moss, bark, leaf litter, soil):** 4–5 colors per tile. Organic materials need one more color step to avoid looking artificial. The extra value goes to a mid-tone transition that softens edges.
- **Fungal/biological surfaces:** 5–6 colors per tile. These are the most chromatically complex surfaces in the game. The additional colors communicate translucency, bioluminescence, or wet-surface specular. This higher budget is a deliberate contrast — biological complexity should be *visually* more complex than human construction.
- **Water/fluid:** 3 colors maximum. Animated through palette cycling, not through tile variation. Simplicity sells the movement.

#### Material Communication at Low Resolution

Each material has a pixel-level signature that must be consistent across every tile in which it appears.

**Concrete:**
- Flat fields of 2 close values (base + slight variation)
- Cracks are single dark pixels in straight or branching lines, never curved
- Rebar exposure = 1-pixel-wide warm-toned (rust orange) lines emerging from crack intersections
- Surface staining reads as a third value applied in gravity-consistent vertical streaks

**Metal:**
- Higher contrast than concrete — darkest darks and a single sharp highlight pixel
- Highlight pixel placement implies curvature or angle; keep it consistent with the light direction of the biome
- Rust is not uniform: it clusters at edges and joints (corners of the tile, not center)
- Intact metal has a 1-pixel specular dot; rusted metal has none

**Wood:**
- Horizontal or vertical grain lines, never diagonal (diagonal grain at this resolution reads as noise)
- Grain = alternating rows of two close values, 1–2 pixels wide
- Rot manifests as grain lines breaking apart — gaps of dark pixels where lines were continuous
- Wet wood shifts the entire value range darker by one palette step

**Glass:**
- Intact glass: 2 colors only — a tinted transparent base and a single diagonal highlight line (1 pixel wide)
- Broken glass: the tile becomes mostly empty/background with 1–3 bright pixels at the frame edges representing shards
- Glass is the only material that allows diagonal pixel lines; this is what distinguishes it from everything else

**Moss/Lichen:**
- Clusters of 3–5 pixels in organic (non-rectangular) shapes
- Never fills a tile uniformly; always leaves gaps showing the substrate beneath
- Minimum two greens per moss cluster (a shadow green and a highlight green)
- Grows from edges inward and from cracks outward — never from the center of an intact surface

**Fungal growth:**
- Rounder forms than moss: 2x2 or 3x3 pixel clusters with single-pixel stalks
- Unique property: fungal tiles are the only surface tiles allowed to use colors outside the biome's assigned sub-palette, pulling from the Fungal/Subterranean palette range. This makes fungus visually "invasive" even in non-fungal biomes.
- Cap highlights use the brightest value in their color column; this implies a slight glow or waxy sheen

**Soil/Earth:**
- The noisiest texture: 3 values distributed semi-randomly with no linear patterning
- This deliberate lack of pattern is what separates soil from all other materials, which have directional structure
- Roots and small stones are single pixels of contrasting value placed sparsely (no more than 2 per 16x16 tile)

#### Tiling Rules

- All environment tiles must be seamless on at least two edges (the two edges that will repeat in their primary orientation)
- Corner tiles and edge tiles are separate assets; do not rely on a single tile to handle both interior and border cases
- Every tileset must include a **minimum of 3 variants** for its primary ground/wall tile to avoid visible repetition. Variants differ by 2–4 pixels only — just enough to break patterning without losing material consistency
- Auto-tile configurations: each tileset must support the standard 47-tile blob autotile layout for Godot's TileMap system. Priority tilesets (those that overlap other biomes) need full 256-tile configurations.

---

### 6.3 Prop Density Rules

#### The Readability Hierarchy

Every pixel on screen falls into one of three layers, and props must respect this hierarchy absolutely:

1. **Gameplay layer (highest priority):** Platforms, hazards, interactables, enemies, the player character. Nothing in the prop layer may obscure or visually compete with this layer. If a prop overlaps a platform edge, the prop loses.

2. **Navigation layer (second priority):** Environmental cues that guide movement — light sources, color shifts, openings, visible paths. Props can contribute to this layer (a lamp that draws the eye toward a passage) but must not contradict it (clutter blocking a visible path the player can actually traverse).

3. **Storytelling layer (third priority):** Props that build the world. This is where density lives, but it is always subordinate to the layers above.

#### Density by Area Type

**Active gameplay corridors (platforming, combat arenas):**
- Prop density: LOW. Maximum 2–3 small props per screen-width of horizontal space.
- Props must be visually distinct from gameplay geometry. If a filing cabinet looks like a platform, it either *is* a platform or it gets cut.
- Background props only — nothing in the foreground layer that might read as an obstacle.
- Rule of thumb: if the player is moving fast through this space, they should see shapes and atmosphere, not individual objects.

**Exploration rooms (puzzle spaces, optional areas, lore rooms):**
- Prop density: MEDIUM. 4–8 props per room, distributed to create visual focal points.
- The player's movement speed is slower here (puzzle-solving, searching), so props can be more detailed and more numerous.
- At least one prop cluster per room that implies the room's former function (see 6.4).
- Props may be in both background and foreground layers to create depth.

**Narrative setpiece rooms (story-critical, one-time-visit spaces):**
- Prop density: HIGH. These rooms can be dense because the player is meant to stop, look, and absorb.
- Maximum 12–15 props, but tightly art-directed. Every prop is placed with intention.
- Foreground framing props are permitted here — objects that partially overlap the camera to create a "peering into" effect.
- These rooms break the normal density rules because they are not repeated. They are handcrafted, not procedural.

**Biome transition corridors:**
- Prop density: GRADIENT. Density decreases as the player moves from one biome into the transition zone, then increases again as the new biome asserts itself. The transition zone itself is sparse — the visual interest comes from the biome blend, not from props.

#### Prop Placement Grammar

- **Gravity is law.** Every prop must have a plausible reason for being where it is. Fallen objects lie on the floor or lean against walls. Intact objects sit on surfaces. Nothing floats unless it is meant to float (fungal spores, web-suspended debris).
- **Cluster, don't scatter.** Props in groups of 2–3 tell a micro-story. A single chair says nothing. A chair next to a desk with a mug on it says "someone worked here." Isolated props are for deliberate visual punctuation, not for filling space.
- **Respect the grid, then break it.** Human-made props (furniture, equipment, containers) are placed on the tile grid or at 45-degree angles to it. Biological props (fallen branches, root masses, fungal clusters) ignore the grid entirely. The contrast between grid-placed and organic-placed props should be visible and deliberate.
- **Parallax depth encoding.** Props in the near-background parallax layer are rendered at full brightness. Props in the far-background layer lose 1–2 value steps (darker, less contrast). This is not atmospheric perspective — it is a simple value shift that tells the player "this is behind you, not beside you."

---

### 6.4 Environmental Storytelling Guidelines

#### The "Last Day" Principle

Every human space in Rewild is a snapshot of its last day of use. Not its best day, not its worst day, but its *last ordinary day* before everything changed. The storytelling power comes from mundanity interrupted. An artist designing a room should first ask: "What was happening in this room at 2 PM on a Tuesday?" Then apply decades of decay to that scene.

#### Room Identity Rules

A room's former function must be communicable within 2 seconds of the player entering it. This is achieved through **three signals**, of which at least two must be present:

1. **Room geometry** (shape, proportions, fixed architectural features like counters, stages, or alcoves)
2. **Signature prop** (one object that is iconic to the room type — a chalkboard for a classroom, a gurney for a hospital, server racks for a data center)
3. **Surface treatment** (tile flooring for a bathroom, carpet texture for a residential space, industrial epoxy for a laboratory)

An artist should never rely on only one signal. A gurney in a featureless box is ambiguous. A gurney in a narrow room with curtain-track hardware on the ceiling and linoleum flooring is unmistakably a hospital corridor.

#### Room Type Reference Chart

| Room Type | Geometry Signal | Signature Props | Surface Signal |
|---|---|---|---|
| Classroom | Wide, shallow, one flat wall | Desks (rows), board surface | Hard flooring, one wall lighter |
| Office | Modular, divided, drop ceiling | Desk+chair+monitor, filing cabinet | Carpet tile, fluorescent fixtures |
| Server Room | Narrow, deep, raised floor tiles | Rack units, cable bundles | Raised floor, no windows |
| Residential | Irregular rooms, varied ceiling height | Bed frame, kitchen counter, bathtub | Mixed flooring per sub-room |
| Hospital | Wide corridors, small branching rooms | Gurney, curtain tracks, IV stand | Linoleum, wall-mounted fixtures |
| Laboratory | Medium rooms, bench-height counters | Fume hood frame, sink basins | Epoxy floor, safety markings |
| Retail | Open floor, large window openings | Shelving units, checkout counter | Patterned commercial floor |
| Transit | Linear, tiled, platform edges | Benches, signage frames, turnstiles | Ceramic tile, platform edge markings |
| Industrial | Tall ceiling, heavy floor | Machinery bases, conveyor frames, vats | Scored concrete, drainage channels |
| Data Center | Low ceiling, cold aisle/hot aisle layout | Rack units (denser than server room), raised floor vents | Perforated floor tiles, cable trays |

#### Narrative Object Placement

Narrative objects are props that tell a specific, discoverable story. They are distinct from atmospheric props (which set the mood) and functional props (which serve gameplay).

**Rules for narrative objects:**

1. **One story per room maximum.** If a room contains a narrative-significant arrangement, do not compete with it by adding a second unrelated narrative. Clutter the room with atmospheric props, but let the narrative breathe.

2. **Triangulation.** The most effective narrative placements use three objects that the player's eye connects into a story. Examples:
   - A child's shoe + a high water line on the wall + a barricaded door = flooding, someone tried to save a child.
   - An overturned desk + shell casings on the floor + a cracked window = someone took cover, fired outward.
   - A ring of chairs + a pile of emptied containers + blankets = a group sheltered here together at the end.

3. **Never explain.** No journals, no signs with text, no audio logs. Rewild's stories are told through *objects and their spatial relationships*. If you cannot tell the story through arrangement alone, simplify the story until you can.

4. **Decay respects narrative.** When applying decay to a narrative arrangement, preserve the key objects long enough for the story to remain legible. Metal and stone last. Paper and fabric do not. Choose narrative objects made from durable materials, or place the narrative in a sheltered space where decay is slower.

5. **Nature comments on the narrative.** The biological growth in a narrative room should, where possible, add a layer of meaning. Flowers growing from a grave site. Fungus consuming a stockpile of medicine. A tree root lifting a barricade that someone died to hold. Nature does not care about human tragedy, and that indifference is the commentary.

#### The "Ghost" Technique

For high-impact rooms, use the ghost technique: compose the props as if the humans were still present, then remove the humans. A table set for dinner. A row of computers with chairs pushed back. A queue of footwear inside a threshold. The negative space where the person should be creates more emotional resonance than any prop can alone. The player fills in the ghost. Let them.

---

### 6.5 Biome Visual Identity

#### Biome 1: Fresh Ruin

*The most recently abandoned spaces. Architecture is mostly intact. Nature is probing, testing, finding the cracks.*

**Dominant tileset materials:**
- Era 2 and Era 3 architecture in good structural condition
- Intact drywall, cracked but present glass, surface rust on metal (not structural corrosion)
- Concrete with hairline fractures, not chunks missing
- Dust accumulation on horizontal surfaces (rendered as a 1-pixel-high lighter value strip on top edges of props)

**Vegetation/growth type:**
- Pioneer species: grasses pushing through floor cracks, single-stem weeds in window frames
- Thin vine tendrils on exterior walls — not yet load-bearing, just climbing
- Moss only in the dampest areas (bathrooms, basements, north-facing walls)
- No trees, no canopy, no flowers. It is too early.

**Light behavior:**
- Light enters through windows and open doors normally; architecture still controls light flow
- Interior spaces are dim but not dark — enough ambient light bounces off intact surfaces
- Dominant light temperature: warm (late afternoon feel) due to dust particles in the air
- Artificial light sources (emergency LEDs, battery backups) may still be flickering in rare instances — these are the last electronic lights in the game

**Signature visual element:**
- **Dust motes.** Small single-pixel particles drifting slowly in beams of light entering through windows. This is the only biome with visible airborne particles, and they are warm-toned. Implemented as a particle effect layer, not baked into tiles.

**Decay stage indicators:**
- Stage 1 (earliest): Dust and silence. Objects are where they were left. Surfaces are dulled but intact.
- Stage 2: First cracks. Water stains on ceilings. Initial plant penetration at joints and seams.
- Stage 3 (latest, transitioning to Mid-Decay): First structural compromises. A collapsed drop ceiling. A buckled floor section. Vegetation thickening at entry points.

---

#### Biome 2: Mid-Decay

*The balance point. Architecture and nature share the space in an active negotiation. Neither has won.*

**Dominant tileset materials:**
- Era 1 and Era 2 architecture with significant weathering
- Exposed structural elements where cladding has fallen — visible I-beams, rebar grids, concrete cores
- Extensive water damage: staining, mineral deposits, erosion channels
- Mixed surfaces: every wall is part original material, part biological colonization

**Vegetation/growth type:**
- Established shrubs and saplings, some reaching 2–3 tiles in height
- Dense moss carpets on all horizontal surfaces that receive moisture
- Climbing plants with structural mass — thick vines that are pulling at facades, roots in walls
- Leaf litter accumulation on floors (a dedicated ground-cover tile with warm browns and ochres)
- First appearance of small flowers — purple and white, never red or yellow (those are reserved for Fully Reclaimed)

**Light behavior:**
- Light is filtered and broken. Windows are partially occluded by growth, creating dappled light patterns on floors
- Implement dappled light as a semi-transparent overlay tile (2–3 variants) placed on the light layer
- Green tint to all light that passes through vegetation — shift the light overlay 1 palette step toward green
- Interior spaces alternate between bright patches and deep shadow; contrast is higher than any other biome

**Signature visual element:**
- **The vine-crack network.** Walls in Mid-Decay have a visible pattern of cracks filled with vine growth, rendered as dark crack lines with green pixels threaded through them. This motif appears on every vertical surface and is unique to this biome. It visually encodes the "negotiation" — structure and biology occupying the same space at the pixel level.

**Decay stage indicators:**
- Stage 1: Architecture still defines the space, but every surface shows biological presence. "The building has tenants."
- Stage 2: Structural ambiguity — some walls are as much vine as concrete. The eye cannot immediately separate built from grown. "Who is supporting whom?"
- Stage 3: Architecture is losing. Floors sag, walls bow outward, ceilings open to sky. Biology is weight-bearing. Transitioning to Fully Reclaimed.

---

#### Biome 3: Fully Reclaimed

*Nature has won. Architecture exists as substrate, not structure. This is a forest that happens to contain concrete bones.*

**Dominant tileset materials:**
- Remnant architecture only — foundations, column stumps, floor slabs used as terraces by root systems
- All surfaces are biological: bark, soil, leaf matter, moss, lichen
- Human materials appear as fragments: a tile of floor visible beneath soil, a section of wall used as a root brace, rebar protruding from a tree trunk that grew around it
- Stone and concrete are the only human materials still recognizable; metal is deeply corroded, wood is gone, glass is gone

**Vegetation/growth type:**
- Full canopy: large trees (trunks 2–3 tiles wide) with overhead leaf cover
- Understory: ferns, dense ground cover, fallen logs hosting secondary growth
- Flowers in full spectrum — this biome has the widest vegetation color range, including reds and yellows
- Epiphytes on every vertical surface: shelf fungi, orchid-like growths, hanging moss
- Root systems are architectural: they form arches, bridges, walls. Nature's own Metroidvania geometry.

**Light behavior:**
- Canopy-filtered: light arrives in shafts through gaps in the leaf ceiling, with the rest in deep green-tinted shade
- Light shafts are rendered as 2–3 tile-wide vertical bright zones with soft edges (1-pixel gradient falloff on each side)
- Ground level is dim; the color palette compresses toward its cooler values
- No artificial light sources whatsoever. All light is solar, filtered through biology.
- Time-of-day sensitivity is highest here: dusk in Fully Reclaimed should be near-black

**Signature visual element:**
- **The ruin-tree.** At least one per major Fully Reclaimed area: a large tree that has grown *through* a human structure, incorporating it. The trunk splits a floor slab. Roots grip a foundation wall. Branches extend through what was once a window. This is the single most important environmental motif in Rewild — the visual thesis of the game. Ruin-trees are handcrafted setpiece assets, never tiled.

**Decay stage indicators:**
- In Fully Reclaimed, "decay" is reframed as "succession." There are no decay stages here — instead, there are *ecological maturity* stages:
- Young: Vigorous growth, dense and competitive. Many species, much visual variety. Bright greens.
- Mature: Fewer, larger organisms. Canopy is high. Understory is open. Deep greens and browns.
- Old-growth: Massive trees, thick silence, shafts of light. Mossy, ancient feeling. The most saturated cool tones in the game.

---

#### Biome 4: Fungal / Subterranean

*Below the root line. Where decay itself has become an ecosystem. The rules of light and growth invert.*

**Dominant tileset materials:**
- Era 1 architecture (deepest, oldest) — tunnels, sewer systems, utility corridors, sub-basements
- Concrete and stone, permanently damp, mineral-stained
- Mycelium mats covering large surface areas — rendered as a fine web of pale pixels over dark substrates
- Standing water on floors (animated 3-color tiles with palette cycling)
- Organic detritus: decomposing matter that has washed or fallen down from above

**Vegetation/growth type:**
- No photosynthetic plants. Zero green. This is the only biome with no chlorophyll.
- Fungi dominate: shelf fungi on walls, mushroom clusters on floors, hanging fungal structures from ceilings
- Fungal color range: pale whites, violets, deep blues, amber-orange for bioluminescent species
- Mycelium networks visible on surfaces — thin branching lines of pale pixels, 1 pixel wide
- Slime molds in bright warning colors (yellow-orange) in hazardous areas — these double as gameplay hazard indicators
- Roots from above penetrate ceilings, providing vertical organic geometry

**Light behavior:**
- No natural light enters. Darkness is the default state.
- Light sources are exclusively biological: bioluminescent fungi provide the illumination
- Bioluminescent light is cool-toned (blue-violet) with a small radius — each source lights a 3–5 tile radius with steep falloff
- The player's own light source (ability-dependent) is the primary illumination in many areas
- "Glow pools" — patches of bioluminescent fluid in floor depressions, casting light upward (reverse of every other biome's top-down lighting). This under-lighting creates unfamiliar shadow directions and is a key tool for making this biome feel alien.

**Signature visual element:**
- **The spore column.** Vertical shafts where fungal growth has created floor-to-ceiling structures of layered shelf fungi, resembling alien architecture. These are 3–5 tiles wide, organic in form, and glow faintly from within. They serve as both visual landmarks and light sources. No other biome has structures built by a non-human organism at this scale.

**Decay stage indicators:**
- Fresh: Recently submerged or exposed infrastructure. Mycelium is sparse, thin. Architecture is clearly readable.
- Colonized: Fungal growth covers 30–50% of surfaces. Architecture is visible but secondary. Air (represented by particle effects) contains visible spores — single bright pixels drifting upward.
- Consumed: Fungal growth covers 70%+ of surfaces. Original architecture is barely discernible. The space feels biological, not architectural. Surfaces are soft-looking (rounded pixel forms replace angular ones).

---

#### Biome 5: Thermal / Industrial

*Where human industrial infrastructure intersects with geothermal energy. Hot, pressurized, unstable. The earth itself is reclaiming these spaces.*

**Dominant tileset materials:**
- Era 1 heavy industrial: thick walls, reinforced structures, pipe networks, pressure vessels
- Metal dominates — this is the most metallic biome. Heavy riveted plate, grating, conduit
- Thermal damage: warped metal, heat discoloration (amber-to-blue oxide tints on steel surfaces)
- Mineral deposits from geothermal water: white and yellow crystalline crusts on pipes, walls, and floors
- Active water: steam (particle effect), condensation on surfaces (specular pixel on every metal tile), hot springs (animated warm-palette water tiles distinct from Fungal biome's cool water)

**Vegetation/growth type:**
- Extremophile biology only: heat-tolerant bacterial mats in orange and red
- Mineral-depositing microorganisms creating tufa-like structures on pipes and walls
- Thermophilic moss in a unique red-brown palette — the only biome with warm-toned moss
- No large plants, no fungi (too hot). Biology here is microbial and mineral.
- Near cooler zones at the biome's edges: tough ferns and heat-tolerant grasses, signaling the boundary

**Light behavior:**
- Dual light sources: residual industrial lighting (amber, harsh, directional — from still-hot elements and lava-adjacent glow) and geothermal glow (deep orange-red from below)
- This is the only biome with warm-toned primary lighting. All other biomes trend cool.
- Steam partially occludes visibility — rendered as an animated semi-transparent overlay that drifts across the screen, reducing contrast in affected areas
- Specular highlights on metal are pronounced here: 1-pixel bright white dots on every metallic surface, suggesting condensation and heat shimmer
- Deep areas glow from below (magma/geothermal vents), using the hottest values in the master palette — this is where the palette's oranges and reds live

**Signature visual element:**
- **The pipe organ.** Dense networks of industrial pipes that have been colonized by mineral deposits, creating formations that resemble pipe organs or geological columns. They drip, they steam, they have been transformed from functional infrastructure into something that looks geological. The fusion of industrial and geothermal is visible at the pixel level: straight pipe geometry encrusted with organic mineral forms.

**Decay stage indicators:**
- Pressurized: Infrastructure is stressed but holding. Pipes groan (audio cue), surfaces are hot (heat shimmer particle effect), but structures are intact. Visually clean metal with heat discoloration.
- Breached: Pipes have burst, releasing steam and mineral-laden water. Deposit buildup is moderate. Some areas are flooded with hot water. Metal is corroding rapidly.
- Geological: The industrial origin is buried under mineral deposits. Pipes are unrecognizable under tufa. The space looks like a cave that happens to have straight lines underneath. The earth is winning.

---

### 6.6 Transition Zones

#### Philosophy

Biome transitions in Rewild are never arbitrary. Every border between biomes has a *physical reason* — a change in elevation, a breached wall, a water table shift, a temperature gradient. The player should be able to look at a transition and understand *why* the environment changes here.

#### Transition Method: The Five-Screen Gradient

All biome transitions follow a five-screen gradient structure. One "screen" equals one full camera viewport of horizontal or vertical space.

**Screen 1 (Origin biome, full identity):**
- 100% origin biome tileset and palette
- Props and vegetation are fully characteristic of the origin biome
- No visual indication of the coming change

**Screen 2 (First signal):**
- 90% origin biome, 10% destination biome
- The first signal is always **environmental, not tileographic.** It is a shift in light color, a change in ambient particles, or a single prop from the destination biome appearing at the edge of the screen.
- The player registers this subconsciously. "Something is different."
- Rule: the first signal must be perceivable but not nameable. The player should not yet be able to say "I'm entering the Fungal biome."

**Screen 3 (Active blend):**
- 60% origin, 40% destination
- Tiles from both biomes are present. The origin biome's tiles appear at the top/back of the space; the destination biome's tiles appear at the bottom/front.
- Vegetation and growth from both biomes overlap, but destination biome growth is "winning" — it is on top of, or growing over, origin biome growth.
- Light is mixed: both biomes' light sources are active, creating unusual color combinations found nowhere else in the game. This makes transition zones visually unique and memorable.
- A dedicated set of **hybrid tiles** exists for each biome pair that commonly borders. These are not mix-and-match from two tilesets — they are purpose-built tiles that show one material transforming into another (e.g., a concrete wall tile where the left half is vine-covered Mid-Decay and the right half is fungal-colonized Subterranean).

**Screen 4 (Destination dominance):**
- 20% origin, 80% destination
- The destination biome's tileset, palette, and lighting are dominant
- Remnants of the origin biome are visible as holdovers — a few tiles, a lighting echo, a straggling plant from the previous zone
- These remnants serve as a visual "memory" that reinforces the feeling of having traveled

**Screen 5 (Destination biome, full identity):**
- 100% destination biome
- Transition complete. No trace of the origin remains.

#### Required Transition Pair Assets

Each biome borders a specific set of other biomes. The following pairs require dedicated hybrid tilesets:

| Transition Pair | Hybrid Concept | Key Visual |
|---|---|---|
| Fresh Ruin to Mid-Decay | Accelerating growth | Intact walls with one side clean, one side vine-covered |
| Mid-Decay to Fully Reclaimed | Structural surrender | Walls crumbling as root systems push through |
| Mid-Decay to Fungal/Subterranean | Descent into dark | Floors giving way, roots descending, light dimming |
| Fresh Ruin to Thermal/Industrial | Heat intrusion | Clean surfaces developing heat stains, steam appearing |
| Fungal/Subterranean to Thermal/Industrial | Wet to hot | Mycelium dying back, mineral deposits replacing organic growth, water steaming |
| Fully Reclaimed to Fungal/Subterranean | Canopy to cavern | Tree roots becoming cave ceiling, soil becoming stone, green light becoming blue |

#### Transition Audio-Visual Sync

While audio is not this document's domain, the environment artist should know: every transition zone has a unique ambient sound blend. Design visual transitions with the understanding that the player will also *hear* the shift. Place visual changes at the same pacing as audio changes — the art team and sound team must sync their gradients on a per-transition basis.

#### Hard Borders (Exception Cases)

Some transitions are intentionally abrupt:

- **Sealed doors.** A door between biomes that opens to reveal the new environment in a single frame. Used sparingly for dramatic reveals — typically at story-critical moments. The player steps through and the entire visual language changes at once.
- **Vertical drops.** Falling from one biome into another (e.g., a floor collapse from Mid-Decay into Fungal/Subterranean). The transition happens during the fall — the player passes through the gradient at speed, experiencing it as a blur of changing color and light.
- **Thermal vents.** The border of Thermal/Industrial biome is sometimes a literal heat wall — a screen-wide column of steam and heat shimmer that the player passes through. On one side: the origin biome. On the other: immediate Thermal/Industrial. The vent itself is the transition.

Hard borders always have a strong **threshold marker** — a visual element that frames the moment of crossing. A doorframe. A cliff edge. A vent grate. The player should always know the exact pixel where they crossed over.

---

### Production Notes

#### Asset Priority for Environment Art

When building out environments, the tiling order should be:

1. Ground and wall tiles for all 5 biomes (core tilesets)
2. Transition hybrid tiles for all 6 required pairs
3. Signature visual elements for each biome (ruin-trees, spore columns, pipe organs, vine-crack networks, dust mote particles)
4. Prop sets organized by room type (see 6.4 chart)
5. Narrative object sets (modular, recombinable)
6. Lighting overlay tiles and particle effect assets

#### Consistency Checks

Before any environment screen ships:
- Verify that tile color counts do not exceed the budgets in 6.2
- Confirm that at least 2 of 3 room identity signals (6.4) are present for any identifiable space
- Confirm that prop density falls within the ranges defined in 6.3
- Verify that the Decay Gradient reads correctly: warm/muted in the human-built elements, cool/saturated in the biological elements, within the same screen
- Test that transition zones read correctly at gameplay speed, not just when standing still

---

*"Every pixel should look like nature solving a problem." If a tile, a prop, or a transition does not serve that principle, it does not belong in Rewild.*

---

## 7. UI/HUD Visual Direction

### 7.0 Governing Principle

There is no HUD. The world is the interface. Every piece of information the player needs is communicated through the creature's body, the environment's behavior, or the physics of the world itself. If a piece of UI cannot be justified as something the creature would perceive, it does not exist.

This is not a stylistic flourish — it is load-bearing design. The fourth pillar ("You Become What You Survive") requires the player to read their own body the way an animal reads its own pain. A health bar externalizes that knowledge and kills the loop.

---

### 7.1 Health Communication: Sprite Degradation System

Health is not a number. It is visible damage to the creature sprite.

**Degradation Stages (4 tiers):**

| Tier | HP Range | Visual Effect |
|------|----------|---------------|
| **Intact** | 100–76% | Full sprite, all colors at palette-correct saturation. Dorsal ridge animation plays at full frequency (idle shimmer every 90 frames). |
| **Stressed** | 75–51% | Outermost pixel layer loses 1 saturation step. Dorsal ridge shimmer slows to every 140 frames. One limb animation drops 1 frame (subtle limp). |
| **Wounded** | 50–26% | Two saturation steps lost across body. Dorsal ridge goes static (no shimmer). Movement trail shortens by 50%. One pixel of the silhouette boundary "chips" — a single pixel gap appears in a limb or tail edge. Idle animation gains a flinch frame. |
| **Critical** | 25–1% | Three saturation steps lost — creature approaches grayscale. Dorsal ridge pixels flicker erratically (1-frame randomized on/off per ridge pixel). Silhouette visibly degraded: 2–3 pixel gaps in outline. Movement animation loses 2 frames (lurching). A single trailing pixel detaches during movement, fading over 8 frames — the creature is literally losing itself. |

**Rules:**
- Degradation is **per-pixel procedural**, not pre-drawn sprite swaps. Each damage instance selects affected pixels from a weighted map (extremities first, core last, dorsal ridge last of all).
- Recovery reverses the process at 1 saturation step per 3 seconds of safe rest, giving the player a reason to notice and value the restoration.
- Death is not a screen — it is the last pixel of the dorsal ridge going dark, followed by 60 frames of the remaining body pixels scattering outward like spores. No fade to black. The camera holds position (Uninvited Frame) and the world continues for a full 2 seconds before respawn.

---

### 7.2 Navigation: Environmental Wayfinding

No minimap. No compass. No waypoint markers.

**Primary Navigation Systems:**

1. **Light Direction** — Bioluminescent growth is denser toward biome interiors and sparser toward biome boundaries. The player learns to read light density the way a hiker reads tree density. Critical paths have slightly higher ambient luminosity (never a spotlight — a 5–8% brightness increase in the background layers, detectable but not obvious).

2. **Water Flow** — All water in Rewild flows. Direction of flow is consistent per biome and always moves toward the biome's ecological center. Water is the closest thing to a compass the game offers. Puddles, drips, condensation trails on walls — all directional.

3. **Root and Vine Directionality** — Organic growth follows structural logic. Roots grow downward toward nutrient sources (deeper areas). Vines grow upward toward light sources (surface areas). Mycelial threads grow laterally toward connected biomes. The player can read the "intent" of organic structures to infer what lies in each direction.

4. **Creature Traffic** — Ambient fauna moves along paths. Small creatures flee toward shelter. Pollinators move toward food sources. If the player observes before acting, the world's inhabitants reveal the map through their behavior.

5. **Acoustic Gradient** (visual representation) — Distant ambient sounds are represented by subtle 1-pixel particle effects at screen edges: water mist from the direction of water, pollen drift from the direction of dense growth, dust from the direction of ruins. These are environmental, not UI — they exist in the world space and parallax correctly.

**What the player will get lost.** This is intentional. Getting lost forces observation, and observation is the core skill the game teaches.

---

### 7.3 Pause/Menu Design

The pause menu is a **stillness**, not a screen.

**Pause Behavior:**
- When the player pauses, the world does not freeze — it enters a state of deep rest. All creatures settle into idle poses. Wind slows to zero over 20 frames. Particles drift to rest. Water surface goes glassy.
- The camera pulls back by exactly 1 parallax unit, revealing slightly more of the environment — as if the creature's awareness has expanded in stillness.
- The current biome's ambient palette shifts 1 step toward its most saturated value. The world becomes briefly, subtly more vivid. This is the only moment the game "rewards" the player visually for stopping.

**Menu Options** appear as modifications to the environment within camera view:
- **Resume**: Not a button. The player simply unpauses (same input). No visual element needed.
- **Settings**: A small cluster of 3–4 bioluminescent nodes appears near the creature, each pulsing at a different frequency. Selecting one opens a submenu rendered as branching filaments from that node. Audio settings branch from the node that pulses in rhythm with ambient sound. Display settings branch from the brightest node. Controls branch from the node nearest the creature's limbs.
- **Quit**: A fading path of desaturated pixels leading off the nearest screen edge. Following it triggers a confirmation — the path brightens or dims based on the player's input direction.

**No black screens. No overlays. No rectangles.** The menu exists in the world's visual language.

---

### 7.4 Typography Direction

Text is nearly absent. Where it exists:

**In-World Text (Ruin Inscriptions):**
- Human-era text found on ruins uses a **monospaced, degraded typeface** — imagine OCR-A with entropy. Characters are 5x7 pixel grid maximum, rendered in the warm/muted end of the Decay Gradient (rusted oranges, faded yellows).
- Text is never fully legible on first encounter. Biological overgrowth obscures portions. As the player's creature evolves sensory adaptations, more characters become readable — not through a "scan" UI, but because the creature's visual processing literally resolves more pixels of the inscription.
- Inscriptions are environmental objects. They have depth, cast shadows, and degrade across parallax layers like any other ruin element.

**System Text (Mandatory Legal/Accessibility):**
- Title screen, save confirmation, accessibility options: rendered in a **custom pixel font, 5x5 character grid**, using only the 2 most neutral colors from the master palette (the grays that sit between warm and cool).
- Minimal kerning. No flourishes. The font should feel like it was grown, not designed — slight organic irregularity in stroke width (1 pixel variance).
- System text is always bottom-left, small, and fades after 3 seconds of no interaction.

**No floating damage numbers. No pickup labels. No tutorial prompts.** If the player needs to understand something, the world teaches through consequence.

---

### 7.5 The Single Exception: Shelter-Direction Indicator

One element exists outside pure diegesis. It is earned, not given.

**The Dorsal Ridge Compass:**
- A **4x4 pixel indicator** appears in the bottom-right corner of the screen only after the player has established their first shelter (a safe rest point).
- It uses the **silhouette of the creature's dorsal ridge** — the same diagnostic shape that identifies the player creature — rendered as a simplified 4x4 icon.
- The icon is oriented to point toward the nearest shelter. It rotates in 8 increments (N, NE, E, SE, S, SW, W, NW).
- It uses a single color from the master palette: the same color as the creature's dorsal ridge at full health.
- **Degradation rule**: When the creature is at Critical health (25% or below), the indicator flickers in sync with the dorsal ridge's erratic behavior. It becomes unreliable precisely when the player needs it most — the creature is too damaged to orient.
- **Distance encoding**: The indicator's pixel brightness scales inversely with distance. Adjacent to shelter: full brightness. Maximum distance: 1 step above invisible. This is not labeled or explained.
- The indicator has no background, no border, no container. It is 4x4 pixels floating at a fixed screen position with 1 pixel of shadow to ensure readability against any background.

This is the only concession to non-diegetic UI. It exists because shelter is survival, and even injured animals retain a sense of home direction. It is biologically justified.

---

### 7.6 Iconography

Icons exist only in one context: the pause-menu settings submenus (Section 7.3).

**Style Rules:**
- All icons are **5x5 pixel maximum**, single-color, drawn from the master palette.
- Icon shapes are derived from biological forms: a sound wave is a ripple on water. A brightness slider is a bioluminescent node's glow radius. A control remap is a joint/limb diagram.
- No geometric primitives (no circles, no squares, no arrows). Every shape must pass the test: "Could this be a cross-section of something alive?"
- Icons do not use outlines. They are solid fills with 1-pixel internal detail maximum.
- No icon appears more than once. If two functions need indication, they get two distinct biological forms.

---

### 7.7 Evolution Information: The Body As Skill Tree

Evolution is not a menu. It is morphology.

**How the player knows what they've gained:**
- Every evolution modifies the creature's sprite. A new traversal ability adds or modifies pixels on the relevant body part. Wall-cling grows setae (visible as 1-pixel protrusions on limbs). Underwater breathing adds gill slits (2-pixel lines on the thorax). Toxin resistance shifts 2–3 body pixels toward the color of the resisted toxin.
- The creature's silhouette changes. This is how the player "reads" their build — the same way the player reads every other creature's ecological role (Silhouette Ethology principle applied to the self).

**How the player knows what they can gain:**
- They don't, explicitly. They encounter environmental challenges. They survive (or don't). Survival in specific conditions triggers adaptation. The creature's body near relevant hazards shows **1-pixel "stress indicators"**: a limb pixel flickers near a wall the creature cannot yet cling to; a thorax pixel pulses near water the creature cannot yet breathe in.
- These stress indicators use the color of the adaptation they foreshadow. They are the creature's body attempting to solve a problem in real time. If the player persists in that environment, the adaptation resolves and the flickering pixel stabilizes into a permanent addition.

**How the player tracks accumulated evolutions:**
- They look at themselves. The shelter (safe rest point) includes a still-water surface that reflects the creature's sprite at 2x resolution, allowing the player to study their own morphology. This is the game's equivalent of a character sheet — a puddle.


---

## 8. Asset Standards

### 8.0 Scope

These standards govern all pixel art assets produced for Rewild, targeting Godot 4.x on Mac/PC. Every rule here serves two masters: visual consistency with the art bible, and engine performance at 60fps on minimum-spec hardware (integrated GPU, 8GB RAM).

---

### 8.1 Sprite Resolution Standards

| Asset Type | Base Size | Max Size | Notes |
|------------|-----------|----------|-------|
| **Player creature** | 16x16 | 32x32 | Base size at spawn. Max size at full evolution. Growth is continuous across evolutions — intermediate sizes (20x20, 24x24, etc.) are valid. All sizes must maintain dorsal ridge legibility. |
| **Small fauna** (insects, microorganisms) | 8x8 | 12x12 | Must read as a clear silhouette at 8x8. No internal detail below 8x8 — silhouette only. |
| **Medium fauna** (creature-scale organisms) | 16x16 | 32x32 | Same resolution class as the player. Silhouette differentiation from the player is mandatory — no medium fauna shares the dorsal ridge shape. |
| **Large fauna** (apex organisms, bosses) | 32x32 | 64x64 | May be composited from multiple sprite parts for animation efficiency. Core body and appendages as separate sprites if exceeding 48x48. |
| **Mega-structures** (living architecture, overgrown ruins) | 64x64 tiles | 128x128 composites | Built from tileable segments. Never a single monolithic sprite. |
| **Terrain tiles** | 16x16 (primary) | — | Standard tile unit. All terrain authored at this resolution. |
| **Detail tiles** | 8x8 (detail overlay) | — | For small organic details (moss patches, fungal spots, mineral deposits) layered on top of primary tiles. |
| **Parallax elements** | Variable per layer | See Section 8.7 | Background elements authored at lower effective resolution, scaled appropriately per parallax depth. |

**Hard Rule**: No sprite at any resolution may use more than the 32-color master palette. No anti-aliasing against transparency — all edges are hard pixel boundaries.

---

### 8.2 Animation Frame Budgets

Frame budgets are maximum allocations. Artists should use fewer frames when fewer frames read clearly. Every frame must justify its existence.

**Player Creature:**

| State | Frames | Loop | Notes |
|-------|--------|------|-------|
| Idle | 4–6 | Yes | Includes breathing motion and dorsal ridge shimmer. Shimmer is a color cycle, not a shape change. |
| Walk | 6–8 | Yes | Gait must communicate current evolution (limb count, locomotion type). |
| Run/Sprint | 6–8 | Yes | Faster cycle of walk, not a separate animation set. Reduce per-frame hold time, not frame count. |
| Jump (ascend) | 3–4 | No | Anticipation, launch, apex. |
| Fall | 2–3 | Yes | Falling pose + subtle limb flutter. |
| Land | 2–3 | No | Impact compression + recovery. |
| Attack (per type) | 4–6 | No | Wind-up must be readable for fairness. |
| Damage taken | 2–3 | No | Flinch + pixel scatter (procedural, not pre-drawn). |
| Death | 6–8 | No | Dorsal ridge fade + pixel scatter. See Section 7.1. |
| Evolution transition | 8–12 | No | The most frame-expensive animation. Played rarely. Sprite morphs between sizes. |
| Swim | 4–6 | Yes | Distinct from walk. Fluid, undulating motion. |
| Climb/Cling | 3–4 | Yes | Subtle grip-shift loop. |

**NPC Creatures (by size class):**

| Size | Idle | Move | Attack | Death |
|------|------|------|--------|-------|
| Small (8x8) | 2 | 2–4 | 2 | 2 |
| Medium (16–32) | 3–4 | 4–6 | 3–4 | 3–4 |
| Large (32–64) | 4–6 | 4–8 | 4–6 | 4–6 |

**Environmental Animation:**
| Element | Frames | Loop |
|---------|--------|------|
| Water surface | 4 | Yes |
| Foliage sway | 3–4 | Yes |
| Bioluminescent pulse | 3–4 | Yes |
| Mycelial spread (growth event) | 6–8 | No |
| Ruin crumble (triggered) | 4–6 | No |

---

### 8.3 File Format and Naming Conventions

**Format**: PNG (RGBA, 8-bit). No exceptions. No lossy formats. No indexed-color PNGs — use RGBA and enforce palette compliance via tooling.

**Naming Convention**:
```
[category]_[entity]_[state]_[variant].png
```

| Segment | Rules | Examples |
|---------|-------|----------|
| `category` | Lowercase, singular noun. One of: `player`, `creature`, `tile`, `bg`, `fx`, `ui`, `prop` | `player`, `creature`, `tile` |
| `entity` | Lowercase, underscore-separated descriptor | `mossbeetle`, `rusted_girder`, `mycelial_bridge` |
| `state` | Animation state, lowercase | `idle`, `walk`, `attack`, `damaged` |
| `variant` | Optional. Evolution stage, biome variant, or directional suffix | `stage2`, `wetland`, `east` |

**Examples**:
```
player_base_idle.png
player_base_walk.png
player_evolved_stage3_swim.png
creature_mossbeetle_idle.png
creature_mossbeetle_walk.png
creature_sporecrab_attack.png
tile_wetland_ground_01.png
tile_wetland_ground_02.png
tile_ruin_concrete_cracked.png
bg_canopy_layer3.png
fx_spore_burst.png
prop_shelter_reflective_pool.png
ui_dorsal_compass.png
```

**Directory Structure in Godot Project**:
```
res://assets/
  sprites/
    player/
    creatures/
      small/
      medium/
      large/
    props/
  tiles/
    [biome_name]/
  backgrounds/
    [biome_name]/
  fx/
  ui/
```

---

### 8.4 Texture and Sprite Sheet Organization

**Sprite Sheets**:
- One sheet per entity per animation state. Do not combine walk and idle on the same sheet unless total frame count is under 8.
- Frames arranged **horizontally**, left to right, in chronological order.
- Uniform cell size across the sheet: use the largest frame's bounding box as the cell size. Smaller frames are centered within their cell with transparent padding.
- **1 pixel of transparent gutter** between frames. No zero-gutter sheets — Godot's atlas import can bleed without it.
- Maximum sheet width: 2048px. If an animation exceeds this at its cell size, split into multiple sheets with `_partA`, `_partB` suffix.

**Tileset Atlases**:
- One atlas per biome.
- Organized in a grid with 16x16 cells (primary tiles) or 8x8 cells (detail tiles). Do not mix cell sizes on a single atlas.
- Autotile-compatible variants (47-tile bitmask for terrain) should be laid out per Godot's TileSet editor expectations. Document the bitmask layout in a companion `.tres` annotation.
- Maximum atlas size: 2048x2048.

**Naming for sheets**: Append `_sheet` to the standard filename.
```
player_base_walk_sheet.png
creature_mossbeetle_idle_sheet.png
tile_wetland_ground_atlas.png
```

---

### 8.5 Godot Export and Import Settings

All pixel art assets must be imported with these settings in Godot's import dock. Enforce via `.import` file defaults or a shared import preset.

| Setting | Value | Rationale |
|---------|-------|-----------|
| **Filter** | `Nearest` (no filtering) | Hard pixel edges. Linear/bilinear filtering destroys pixel art. |
| **Mipmaps** | `Off` | Not needed for 2D pixel art; wastes VRAM. |
| **Repeat** | `Disabled` (sprites), `Enabled` (tiling backgrounds) | Prevent unintended wrapping on sprite edges. |
| **Compress Mode** | `Lossless` | No compression artifacts. |
| **Process > Size Limit** | `0` (no limit) | Do not auto-resize. |
| **SVG Scale** | N/A | No SVG assets in this project. |

**Godot Project Settings (enforced globally)**:
- `rendering/textures/canvas_textures/default_texture_filter` = `Nearest`
- `display/window/stretch/mode` = `viewport`
- `display/window/stretch/aspect` = `keep`
- Target viewport resolution: **480x270** (16:9), scaled to display resolution. This gives a pixel density where 16x16 sprites are substantial on screen.

**Sprite node defaults**:
- `texture_filter`: Override to `Nearest` on all Sprite2D/AnimatedSprite2D nodes as a safety net.
- `centered`: `true` for creatures, `false` for tiles.

---

### 8.6 Color Palette Enforcement

The 32-color master palette is law. No asset ships with an off-palette pixel.

**Enforcement Pipeline**:

1. **Palette file**: The master palette exists as a `rewild_palette_32.pal` (JASC-PAL format) and a `rewild_palette_32.png` (32x1 pixel swatch strip). Both live at `res://assets/palette/`.
2. **Art tool setup**: All artists configure their editor (Aseprite, LibreSprite, or equivalent) to use the palette file as the active palette with **no dithering to off-palette colors**. Indexed color mode is recommended during authoring; final export as RGBA PNG.
3. **Automated validation**: A CI/pre-commit script (Python + Pillow) scans every PNG in `res://assets/` and flags any pixel whose RGBA value does not match one of the 32 palette entries (plus fully transparent `#00000000`). Flagged assets block merge.
4. **Palette sub-ranges**: The 32 colors are grouped into functional sub-palettes. Artists working on a specific domain pull from the correct range:

| Sub-Palette | Colors | Usage |
|-------------|--------|-------|
| Warm/Muted (Human Past) | 8 colors | Ruin materials, corroded metal, faded paint, concrete, dust. |
| Cool/Saturated (Biological Present) | 10 colors | Living flora, fauna, bioluminescence, water. |
| Transition | 6 colors | Organic growth over inorganic substrate — mossy concrete, rusted-and-colonized metal. |
| Neutral | 4 colors | Shadows, deep background, system text, silhouette fills. |
| Accent | 4 colors | Dorsal ridge, stress indicators, critical-state flickers, shelter compass. |

5. **No per-biome palette extensions.** Biome identity comes from which sub-palette ranges dominate, not from unique colors. The Wetland is cool-saturated-heavy. The Ruin District is warm-muted-heavy. The Canopy Transition uses the transition range. Same 32 colors, different ratios.

---

### 8.7 Parallax Layer Standards

Five parallax layers, back to front. Each layer has a defined scroll rate, effective resolution, and content type.

| Layer | Scroll Rate | Effective Resolution | Content | Color Range |
|-------|-------------|---------------------|---------|-------------|
| **L0 — Deep Background** | 0.1x camera | Authored at 120x68, displayed at 480x270 (4x upscale, nearest) | Sky, distant horizon, deep geological forms. Minimal detail — shapes and color fields only. | Neutral sub-palette only. Darkest values. |
| **L1 — Far Background** | 0.25x camera | Authored at 160x90, displayed at 480x270 (3x upscale) | Distant structures (mega-ruins, mountain-scale overgrowth). Silhouettes with 1–2 interior detail colors. | Neutral + 1–2 warm-muted or cool-saturated. |
| **L2 — Mid Background** | 0.5x camera | Authored at 240x135, displayed at 480x270 (2x upscale) | Medium-distance environment. Recognizable shapes — trees, collapsed buildings, fungal towers. Internal detail permitted. | Full sub-palette access, but desaturated 1 step from foreground equivalents. |
| **L3 — Foreground (Gameplay)** | 1.0x camera | 480x270 native | All gameplay-relevant geometry: terrain tiles, creatures, player, props, interactable objects. | Full palette access. |
| **L4 — Near Foreground** | 1.2x camera | 480x270 native | Overlapping environmental elements: hanging vines, foreground foliage, drifting particles, rain/spore layers. Must never obscure gameplay-critical information for more than 30 frames continuously. | Cool/saturated + transition sub-palettes. Semi-transparent elements permitted (50% alpha maximum, no intermediate alpha — either full or 50%). |

**Rules**:
- Layers L0–L2 are authored at lower resolution and upscaled. They must not contain detail that would be lost or aliased at their display scale. Artists preview at display scale during authoring.
- No layer shares sprite assets with another layer. A tree in L2 is a different asset from a tree in L3 — different resolution, different detail level, different color treatment.
- Layer L4 uses alpha only for its "veil" elements. No alpha blending on L0–L3. Transparency on those layers is binary (visible or not).
- All parallax layers tile horizontally within their biome. Vertical tiling is not required — biomes have defined vertical extent.

---

### 8.8 Performance Budgets

Target: 60fps on integrated GPU (Intel Iris / Apple M1 base), 480x270 viewport scaled to 1920x1080 display.

| Metric | Budget | Notes |
|--------|--------|-------|
| **Max unique sprites on screen** | 64 | Includes player, creatures, props, animated details. Does not count tiles or parallax layers. |
| **Max animated sprites simultaneously** | 24 | Subset of the 64 above. Remaining sprites hold a static frame. Off-screen sprites beyond 1.5x viewport width halt animation entirely. |
| **Max particle emitters active** | 6 | Spore drifts, water mist, dust — maximum 6 concurrent emitters. |
| **Max particles per emitter** | 32 | Particles are 1x1 or 2x2 pixels. No larger. No texture-based particles — all are palette-colored rects. |
| **Total particles on screen** | 128 | Hard cap across all emitters. If a new emitter would exceed this, the oldest particles from the lowest-priority emitter are culled. |
| **Tile layers per screen** | 3 | Ground, detail overlay, collision-relevant foreground. A fourth decorative layer is permitted if total visible tile count stays under budget. |
| **Max visible tiles** | 600 | Across all tile layers. At 16x16 tiles in a 480x270 viewport, a single full-screen layer is 30x17 = 510 tiles. Two full layers exceed budget — use sparse detail layers, not full coverage. |
| **Draw calls (target)** | < 120 | Aggressive atlas batching required. One draw call per atlas, not per sprite. |
| **VRAM (texture memory)** | < 128MB | All loaded biome assets, all layers. Biome transitions stream the next biome's assets during traversal corridors (narrow connecting passages exist partly for this reason). |

**Culling Rules**:
- Sprites beyond 1.5x viewport width in any direction: invisible + animation paused.
- Sprites beyond 3x viewport width: fully unloaded, regenerated from scene data on re-approach.
- Parallax layers L0 and L1 use a single pre-rendered image per biome, not tilesets — minimal GPU cost.
- Particle priority (low to high): ambient atmosphere < environmental reaction < creature behavior < player interaction. Lower priority culled first under budget pressure.


---

## 9. Reference Direction

### 9.0 Usage Note

References are surgical. Each one contributes a specific visual lesson. No artist should attempt to make Rewild "look like" any single reference. The game looks like itself — these references calibrate specific axes of that identity.

---

### 9.1 Rain World

**Studio/Creator**: Videocult
**Medium**: 2D game, procedural animation, pixel art

**Draw from this — Creature Locomotion and Weight**:
Rain World's procedural animation gives its creatures a physical presence that frame-by-frame animation rarely achieves. Every creature moves like it has mass, joints, and friction against surfaces. Study how Slugcat's body compresses on landing, how lizards' limbs find footholds independently, how pole-plants sway with physically-grounded sine curves.

For Rewild, apply this thinking to the player creature and all medium-to-large fauna. Animations must communicate weight and surface interaction even within strict frame budgets. The diagnostic question for every walk cycle: "Can I tell what this creature weighs and what surface it's standing on with the sound off?"

**Explicitly avoid**: Rain World's color palette is drab and functionally monochromatic in many biomes. Rewild's biological present is saturated and chromatically diverse — the Decay Gradient demands that living things announce themselves through color. Also avoid Rain World's difficulty communication style (or lack thereof); Rewild's diegetic UI must be more legible than Rain World's, which often leaves players with genuinely zero information.

---

### 9.2 Hollow Knight

**Studio/Creator**: Team Cherry
**Medium**: 2D game, hand-drawn animation

**Draw from this — Silhouette Hierarchy and Negative Space**:
Hollow Knight's creature design is masterful at communicating scale, threat, and role through silhouette alone. The Mantis Lords read as "fast, angular, predatory" before a single attack animation plays. The Moss Knight reads as "heavy, patient, armored." This is achieved through disciplined use of negative space: the gap between limbs, the ratio of body to appendage, the symmetry or asymmetry of the outline.

For Rewild, apply this to the Silhouette Ethology principle. Every creature's ecological role — predator, decomposer, symbiont, parasite — must be legible from its silhouette at minimum zoom. The 8x8 small-fauna test: if the creature is 8x8 and silhouette-only, can the player guess its behavior?

**Explicitly avoid**: Hollow Knight's UI is beautifully designed but entirely non-diegetic (soul meter, mask health, charm notches). Do not import any of its UI thinking. Also avoid its environmental legibility — Hollow Knight's backgrounds often clearly separate interactive and non-interactive layers through art style, which undermines Rewild's Uninvited Frame principle. In Rewild, the environment should not visually "help" the player distinguish gameplay surfaces from decoration.

---

### 9.3 Nausicaa of the Valley of the Wind (Manga)

**Creator**: Hayao Miyazaki
**Medium**: Manga (ink on paper), 7 volumes, 1982–1994

**Draw from this — The Visual Language of Ecosystem Reclamation**:
The Nausicaa manga (not the film — the manga) is the single best visual reference for how nature consumes human infrastructure at civilizational scale. Miyazaki draws the Toxic Jungle not as hostile wilderness but as a functioning purification system — every fungal tower, every insect swarm, every spore cloud serves an ecological purpose that the art communicates through form. The jungle's visual logic is internally consistent: structures branch and root according to implied biological rules.

For Rewild, this is the reference for biome macro-design. When a ruin is overgrown, the overgrowth should look purposeful. Roots follow structural cracks because that's where water collects. Fungal blooms cluster on organic-rich surfaces (old wood, old fabric, old bodies). Vine coverage correlates with sun exposure. The overgrowth reads as an ecosystem solving the problem of decomposing a dead civilization.

**Explicitly avoid**: Miyazaki's human-scale storytelling. His compositions center human (or humanoid) figures and their emotional responses to nature. Rewild's camera does not privilege its protagonist (Uninvited Frame), and the creature is not humanoid. Also avoid the manga's scale — Miyazaki draws continent-scale vistas. Rewild's world is intimate, 480x270, and the grandeur must come from density and detail, not panoramic sweep.

---

### 9.4 Death Stranding

**Studio/Creator**: Kojima Productions
**Medium**: 3D game, photorealistic rendering

**Draw from this — Environmental Storytelling Through Decay Stratigraphy**:
Death Stranding renders the passage of time as a visible material process. Timefall ages surfaces in real time — metal corrodes, fabric tears, moss colonizes. More importantly, the game layers these decay states: you can read the history of an object by its surface. A building with rusted steel beams, shattered glass colonized by lichen, and fresh BT-corruption on top tells a three-era story in a single prop.

For Rewild, translate this into pixel art stratigraphy. Every ruin prop should encode at least two eras in its pixel composition: the human-era material (warm/muted palette) and the biological present (cool/saturated palette), with a visible boundary between them. A concrete wall is warm gray at its core, transition-palette mossy green at its surface, and cool-saturated vine over the moss. Three layers, three eras, readable at 16x16.

**Explicitly avoid**: Death Stranding's photorealism, obviously, but also its environmental emptiness. Its landscapes are vast and barren to serve a loneliness thesis. Rewild's world is teeming. Every surface hosts life. The quiet takeover is not empty — it is so full that human absence becomes the notable gap.

---

### 9.5 Ernst Haeckel — *Kunstformen der Natur* (Art Forms in Nature)

**Creator**: Ernst Haeckel
**Medium**: Lithographic prints, 1904

**Draw from this — Biological Ornamentation as Structural Logic**:
Haeckel's illustrations of radiolaria, medusae, and other organisms demonstrate that biological forms are ornate because ornamentation is structural. A radiolarian's fractal silica skeleton is not decorative — it is an engineering solution to the problem of being microscopic and needing to float. The visual complexity communicates function.

For Rewild, apply this to all evolved biological structures in the game. When the world builds bioluminescent lattices across a ruin ceiling, the lattice pattern should follow structural logic (load-bearing, nutrient-distributing, light-catching). When a creature evolves a complex appendage, its visual complexity should communicate mechanical function, not aesthetic elaboration. Reference Haeckel's plates directly when designing fungal architectures, coral-like growth formations, and the player creature's evolved morphologies.

**Explicitly avoid**: Haeckel's symmetry. His illustrations emphasize bilateral and radial symmetry because he was classifying organisms. Rewild's overgrown world is asymmetric — growth follows opportunity, not geometry. Also avoid his clinical white backgrounds. Every organism in Rewild exists in context, on a surface, in a biome. No specimen-plate isolation.

---

### 9.6 Celeste

**Studio/Creator**: Maddy Makes Games (Matt Thorson, Noel Berry)
**Medium**: 2D game, pixel art

**Draw from this — Pixel-Level Legibility Under Motion**:
Celeste solves a problem Rewild shares: fast, precise gameplay in dense pixel art environments. Its visual clarity under extreme movement speed is unmatched in the medium. Study how Celeste maintains player legibility through: (a) consistent character brightness relative to background, (b) motion trails that reinforce trajectory without cluttering the scene, (c) foreground elements that never ambush the player's visual tracking.

For Rewild, the player creature must remain instantly identifiable during fast traversal, combat, and environmental chaos (collapsing ruins, swarm encounters, spore storms). The dorsal ridge is the identification anchor — it must maintain contrast against any biome background. Celeste's trail-particle approach (afterimage with rapid fade) is directly applicable to Rewild's movement, adapted to the diegetic rule: the trail is shed bioluminescent residue, not an abstract effect.

**Explicitly avoid**: Celeste's color philosophy. Its palette is emotionally coded (anxiety = red/dark, safety = blue/warm) in service of a human psychological narrative. Rewild's palette is ecologically coded (Decay Gradient). Do not map emotions to colors — map eras and biological processes to colors. Also avoid Celeste's UI generosity (stamina indicator, screen-edge markers, death counter). Every one of those is a non-diegetic concession Rewild does not make.

---
