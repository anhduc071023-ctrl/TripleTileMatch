# Triple Tile Match

**Casual Puzzle Game | Game Design | Unity 6 | C#**

## Table of Contents

- [Game Overview](#1-game-overview)
- [Game Design Pillars](#2-game-design-pillars)
- [Core Gameplay Loop](#3-core-gameplay-loop)
- [Gameplay Mechanics](#4-gameplay-mechanics)
- [Power-up System](#5-power-up-system)
- [Level Design](#6-level-design)
- [Difficulty Balancing](#7-difficulty-balancing)
- [Player Progression](#8-player-progression)
- [UI/UX Design](#9-uiux-design)
- [Art & Audio Design](#10-art--audio-design)
- [Technical Implementation](#11-technical-implementation)
- [Testing & Quality Assurance](#12-testing--quality-assurance)
- [Monetization & Future Development](#13-monetization--future-development)
- [My Contributions](#14-my-contributions)

---

## 1. Game Overview

**Triple Tile Match** is a single-player casual puzzle game for mobile devices, built around a triple-tile matching mechanic.

Players must clear a layered board by selecting available tiles and placing them into a limited-capacity tray. When three tiles with identical icons are collected, they are automatically matched and removed.

The main challenge comes from managing the tray while deciding which tiles to select. Some tiles are blocked by tiles on higher layers, requiring players to plan their selections carefully.

**Game Information**

| Category | Details |
|---|---|
| Genre | Casual Puzzle / Triple Tile Matching |
| Platform | Android (Mobile) |
| Game Mode | Single Player |
| Engine | Unity 6 |
| Programming Language | C# |
| Current Content | 10 Playable Levels |
| UI Orientation | Portrait |

## 2. Game Design Pillars

The game focuses on four core design principles:

### Readable Decisions
- Clearly distinguish available and blocked tiles.
- Use recognizable tile icons.
- Encourage players to observe the board before making decisions.

### Satisfying Gameplay
- Provide visual and audio feedback for every selection.
- Use satisfying animations when three matching tiles are removed.
- Make tray rearrangement clear and responsive.

### Strategic Challenge
- Require players to manage limited tray capacity.
- Create decisions through layered tile arrangements.
- Encourage planning rather than random selection.

### Mobile-friendly Experience
- Simple tap-based controls.
- Accessible UI elements.
- Responsive layouts for different mobile screen ratios.
- Short gameplay sessions suitable for casual players.

## 3. Core Gameplay Loop

**Observe → Select → Match → Clear Tiles → Progress → Win / Lose**

1. Enter a level and observe the tile board.
2. Identify available tiles that are not blocked by higher layers.
3. Select a tile and move it into the tray.
4. The game checks for three tiles with identical IDs.
5. Matching tiles are automatically removed.
6. Continue selecting tiles while managing the remaining tray space.
7. Clear the entire board to win or fill the tray without a valid match to lose.

## 4. Gameplay Mechanics

### 4.1. Tile Selection

- Each tile contains an icon representing its tile type.
- Players select tiles through touch input.
- Only tiles without blockers on higher layers can be selected.
- Selected tiles move from the board into the tray.
- Blocked tiles are displayed in gray, while available tiles remain white.

### 4.2. Layered Tile System

The board consists of multiple tile layers.

- Higher-layer tiles can overlap and block lower-layer tiles.
- A tile becomes selectable when no higher-layer tiles are blocking it.
- Removing upper tiles gradually reveals previously inaccessible tiles.

**Design Purpose:** Create strategic decisions by requiring players to consider tile accessibility and selection order.

### 4.3. Tray Management

The tray is the main resource-management mechanic.

- Maximum capacity: **8 slots**.
- Each selected tile occupies one slot.
- Tiles with identical IDs are inserted next to existing matching tiles where possible.
- Other tiles shift automatically to maintain the correct order.
- Three matching tiles are automatically removed.

**Design Purpose:** The limited tray creates pressure and encourages players to plan their selections carefully.

### 4.4. Triple Matching

- Three tiles with the same ID form a valid match.
- Matching tiles are removed automatically.
- Remaining tray tiles shift to fill empty slots.
- Match animations, particle effects, and sound effects provide feedback.

### 4.5. Win & Lose Conditions

**Victory**

The player wins when all board tiles and tray tiles have been cleared.

```text
Remaining Board Tiles <= 0
AND
Tray Tiles == 0
```

**Defeat**

The player loses when the tray reaches its maximum capacity without containing a valid triple.

```text
Tray Tiles >= 8
AND
No Tile ID Has 3 Or More Entries
```

## 5. Power-up System

The game includes three power-ups to help players handle difficult situations.

| Power-up | Function | Design Purpose |
|---|---|---|
| Undo | Reverses the latest tile selection | Allows players to recover from unintended selections |
| Hint | Helps identify tiles that can contribute to a triple | Reduces uncertainty and supports decision-making |
| Shuffle | Reorganizes remaining board tiles | Provides a different board arrangement |

### Undo
- Restores the most recently selected tile.
- Returns it to its previous board position.
- Re-enables its interaction and updates the remaining tile count.

### Hint
- Checks tile IDs currently present in the tray.
- Prioritizes IDs that already have matching tiles.
- Searches for compatible tiles on the board.
- Prefers available tiles without blockers.

### Shuffle
- Randomizes the remaining unselected tiles.
- Preserves saved board positions across different layers.
- Updates tile positions, sorting order, and collider states.

## 6. Level Design

### 6.1. Level Structure

Each level is designed as a separate layered tile board.

The main level design parameters include:

| Parameter | Design Purpose |
|---|---|
| Total Tile Count | Controls the overall amount of gameplay |
| Tile Types | Determines the variety of matching decisions |
| Layer Count | Controls board depth and accessibility |
| Tile Overlap | Determines which tiles are blocked |
| Tile Distribution | Influences matching opportunities |
| Tray Capacity | Creates the main risk and decision pressure |
| Power-up Availability | Provides additional recovery options |

### 6.2. Level Design Principles

**Gradual Introduction**
- Start with simple and readable board layouts.
- Introduce layered tiles progressively.
- Avoid overwhelming new players with too many tile types.

**Meaningful Decisions**
- Arrange tiles to encourage players to consider which tiles to reveal first.
- Avoid layouts that make important matching combinations inaccessible.
- Create situations where selection order matters.

**Fair Difficulty**
- Keep tile distribution consistent with the triple-matching rule.
- Avoid layouts that create unavoidable early losses.
- Play-test board arrangements before release.

**Progressive Complexity**
- Gradually introduce deeper layers.
- Increase the number of competing tile types.
- Create more complex overlapping arrangements.
- Increase tray-management pressure over time.

### 6.3. Difficulty Progression Framework

| Stage | Design Direction | Player Challenge |
|---|---|---|
| Early Game | Simple layouts and fewer tile types | Learn tile selection and matching |
| Mid Game | More overlapping tiles and additional tile types | Plan selections and manage tray space |
| Late Game | Deeper layers and more complex arrangements | Make careful decisions and manage risks |

*Note: This table describes the intended difficulty progression framework. Individual level parameters should be documented from the actual level designs.*

### 6.4. Level Authoring

The project contains **10 hand-authored playable levels**.

A custom Unity Editor tool, `toolSpawnTile`, supports board creation from Tilemaps.

The generation process:
1. Read occupied cells from source Tilemaps.
2. Collect available spawn positions.
3. Adjust the total number of positions to be divisible by three.
4. Prepare tile prefabs in matching groups.
5. Randomize tile assignments and spawn positions.
6. Spawn tiles across different floors.
7. Assign layer information, Z depth, sorting order, and parent containers.

The generator ensures that tile counts are divisible by three. However, this does not guarantee that every generated layered board is solvable, so playtesting and layout validation remain necessary.

## 7. Difficulty Balancing

### 7.1. Difficulty Factors

The difficulty of a level is influenced by several interacting factors:

- **Tile Count:** More tiles generally increase the number of decisions required.
- **Tile Variety:** More tile types increase matching complexity.
- **Layer Depth:** More layers create additional blocking relationships.
- **Tile Distribution:** The arrangement of matching tiles affects available choices.
- **Tray Pressure:** The 8-slot tray limits the number of unmatched tiles.
- **Power-up Availability:** Assistance tools provide additional recovery options.

### 7.2. Balancing Strategy

The intended difficulty progression follows three stages:

- **Early Game:** Teach the core mechanics and build player confidence.
- **Mid Game:** Introduce more complex layouts and require better planning.
- **Late Game:** Combine multiple difficulty factors to create greater challenges.

When a level feels too difficult, review:
1. Number of tile types.
2. Number of overlapping layers.
3. Distribution of matching tiles.
4. Accessibility of important tiles.
5. Pressure created by tray capacity.
6. Availability of power-ups.

The goal is to increase challenge without making failure feel unavoidable.

## 8. Player Progression

### 8.1. Level Progression

- Players progress by completing levels.
- Completing a level unlocks the next level.
- Previously unlocked levels remain accessible.
- Players can replay completed levels.

The current implementation supports 10 playable levels.

### 8.2. Power-up Progression

Players have access to three power-ups:

- Undo
- Hint
- Shuffle

Each power-up has a locally stored quantity. The current implementation provides an initial inventory of 10 for each power-up.

### 8.3. Save System

PlayerPrefs is used to store local player data.

| Data | Purpose |
|---|---|
| CurrentLevel | Stores the current selected level |
| LevelPassed | Stores unlocked level progression |
| Amount Undo Skill | Stores available Undo quantity |
| Amount Hint Skill | Stores available Hint quantity |
| Amount Shuffle Skill | Stores available Shuffle quantity |
| IsVibration | Stores vibration preference |
| MusicVolume | Stores music volume |

## 9. UI/UX Design

### 9.1. Main Screens

**Loading Screen**
- Displays loading progress.
- Uses a progress slider and percentage text.
- Loads scenes asynchronously.

**Home Screen**
- Start Game.
- Level Selection.
- Settings.
- Vibration preference.

**Gameplay Screen**
- Current level indicator.
- Main tile board.
- 8-slot tray.
- Remaining tile progress bar.
- Undo, Hint, and Shuffle buttons.
- Pause button.

**Pause Popup**
- Resume.
- Restart.
- Return Home.

**Victory Popup**
- Next Level.
- Replay.
- Return Home.

**Defeat Popup**
- Replay.
- Return Home.

### 9.2. UX Principles

- Keep gameplay information clear and readable.
- Provide immediate feedback for player actions.
- Make important controls easy to access.
- Use animations to communicate tile selection and matching.
- Adapt UI layouts to different mobile screen ratios.
- Keep important UI elements within device safe areas.

## 10. Art & Audio Design

### Visual Direction
- Bright and readable 2D tile icons.
- Clear visual distinction between available and blocked tiles.
- Layer depth communicated through sorting order, Z positions, and visual tinting.
- Gameplay-focused visual presentation.

### Animation
DOTween is used for:
- Tile selection and movement.
- Tray insertion and rearrangement.
- Triple-match removal.
- Shuffle movement.

Particle effects are played when a triple match occurs.

### Audio
- Background music.
- Tile selection sound effects.
- Successful match sound effects.
- Audio Mixer-based music volume control.

### Haptic Feedback
- Light vibration feedback on successful matches.
- Players can enable or disable vibration through settings.

## 11. Technical Implementation

### Technologies & Tools

| Component | Technology |
|---|---|
| Game Engine | Unity 6 |
| Render Pipeline | Universal Render Pipeline (2D) |
| Programming Language | C# |
| UI | Unity UI |
| Text | TextMeshPro |
| Animation | DOTween |
| Physics | Unity 2D Physics |
| Audio | Audio Mixer |
| Version Control | Git / GitHub |

### Main Game Systems

| System | Responsibility |
|---|---|
| GameController | Manages current level state and active tiles |
| LevelController | Loads and registers level prefabs |
| TilebaseController | Handles tile input, blocking detection, and visual states |
| SortControllerRemake | Manages tray insertion, matching, and tile removal |
| GameScene | Handles gameplay HUD and win/lose states |
| SkillManager | Manages Undo, Hint, and Shuffle |
| SkillButtonUI | Handles power-up quantities and availability |
| LoadingSceneController | Manages asynchronous scene loading |
| HomeUIManager / LevelPage | Handles home navigation and level selection |
| AudioManager / VolumeSetting | Manages music and sound settings |
| toolSpawnTile | Supports Tilemap-based level creation |

## 12. Testing & Quality Assurance

### Functional Testing
- Verify that blocked tiles cannot be selected.
- Verify that available tiles can be selected.
- Verify correct tray insertion and tile rearrangement.
- Verify triple-match detection and removal.
- Verify win and lose conditions.
- Verify Undo, Hint, and Shuffle behavior.
- Verify level progression and local persistence.

### Player Experience Testing

Testing should also evaluate:
- Whether the rules are easy to understand.
- Whether board layouts are readable.
- Whether the difficulty increases fairly.
- Whether tray management creates meaningful decisions.
- Whether power-ups are useful in difficult situations.

### Balancing Metrics

| Metric | Evaluation Purpose |
|---|---|
| Level Completion Rate | Measures level completion |
| Failure Rate | Identifies difficult levels |
| Completion Time | Evaluates level pacing |
| Power-up Usage | Evaluates assistance dependency |
| Retry Count | Identifies repeated failure points |
| Player Feedback | Identifies usability and difficulty issues |

*These metrics are proposed evaluation criteria; analytics data is not currently implemented.*

### Iteration Process

**Identify Problem → Analyze Cause → Adjust Design → Playtest Again**

The goal is to improve gameplay clarity, fairness, and player satisfaction through continuous iteration.

## 13. Monetization & Future Development

### Current Monetization Status

The current UI contains ad-reward placeholders for power-up replenishment.

A sample AdMob manager exists in the project, but its implementation is commented out. Advertising is not active in the current build.

### Potential Monetization Features
- Rewarded ads for additional Undo, Hint, or Shuffle uses.
- Optional interstitial ads between completed levels.
- Cosmetic tile themes.
- Ad-free purchase.

### Future Development
1. Develop a solver or validation system to verify generated board solvability.
2. Move level metadata and balancing parameters into ScriptableObjects.
3. Introduce configurable difficulty ratings for individual levels.
4. Complete rewarded-ad integration.
5. Add object pooling for tile instances and particles.
6. Add automated tests for matching, tray insertion, and win/lose conditions.
7. Introduce analytics for level starts, completions, failures, and power-up usage.
8. Explore daily challenges and additional level packs.

## 14. My Contributions

- Designed core gameplay mechanics and the overall gameplay loop.
- Defined tile selection, triple-matching, tray management, and win/lose rules.
- Structured layered board mechanics and level design principles.
- Designed player assistance systems, including Undo, Hint, and Shuffle.
- Planned difficulty progression through tile count, tile variety, layer depth, and tray pressure.
- Designed player progression and local save systems.
- Prototyped and implemented gameplay features using Unity and C#.
- Tested gameplay systems and refined their behavior.

## Project Summary

Triple Tile Match is a Unity 2D casual puzzle game built around layered-tile accessibility, limited tray management, and triple matching.

The project demonstrates practical experience in **Gameplay Design, Puzzle Mechanics, Level Design, Difficulty Balancing, Player Progression, and Game Design Documentation**, supported by hands-on Unity and C# development experience.
