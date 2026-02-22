# SPEC.md - Mini RPG 8-Bit

## Project Overview
- **Project name**: TerraCreatures
- **Type**: 2D Top-down RPG Game
- **Core functionality**: Retro-style monster catching RPG with exploration, turn-based battles, and capture system
- **Target users**: Casual gamers, retro game enthusiasts

## Visual & Rendering Specification

### Scene Setup
- **Canvas**: Single HTML5 Canvas element, 640x480 base resolution
- **Scaling**: Responsive scaling maintaining 4:3 aspect ratio
- **Background**: Dark retro frame around canvas

### Art Style - Game Boy Palette
```
Colors:
- #0f380f (Darkest green - shadows, text)
- #306230 (Dark green - terrain)
- #8bac0f (Light green - highlights)
- #9bbc0f (Lightest green - UI background)
```

### Game States
1. **exploration**: Free movement on map
2. **battle**: Turn-based combat screen
3. **dialogue**: Text box with narrative
4. **menu**: Inventory overlay

## Game Systems Specification

### Map System
- **Grid**: 20 columns x 15 rows
- **Tile size**: 32x32 pixels
- **Tile types**:
  - 0: Grass (encounter zone) - #306230
  - 1: Floor - #8bac0f  
  - 2: Obstacle/Wall - #0f380f
  - 3: Water - #0f380f with wave pattern
- **Camera**: Centered on player, clamped to map bounds

### Player
- **Appearance**: 16x24 pixel character, bald head, simple body
- **Movement**: WASD or Arrow keys, 4 direction facing
- **Animation**: 2-frame walk cycle at 150ms interval
- **Stats**: HP (max 100), Level (starts at 1)

### Creature System
- **6 creature types** with unique stats:
  1. Flameling (Fire) - ATK: 12, HP: 35
  2. Aquapup (Water) - ATK: 10, HP: 40
  3. Leafling (Grass) - ATK: 9, HP: 45
  4. Rockling (Rock) - ATK: 14, HP: 30
  5. Sparkit (Electric) - ATK: 11, HP: 38
  6. Shadewisp (Ghost) - ATK: 13, HP: 32

### Battle System
- **Turn order**: Player first
- **Actions**:
  - Attack: Deal damage (Base ATK ± random 1-4)
  - Capture: Success chance = (enemy max HP - enemy current HP) / enemy max HP * 0.7 + 0.1
  - Run: 70% success chance
- **Damage formula**: attacker.ATK + random(1, 4) - random(1, 2)

### Inventory
- **Capacity**: 6 creature slots
- **Display**: Overlay panel showing creature name, HP, level
- **Access**: Press 'I' key

### Persistence (localStorage)
- **Save data**:
  - Player HP, Level, Position (x, y)
  - Captured creatures array
  - Game state
- **Auto-save**: On battle end, on position change

## Interaction Specification

### Controls
- **Movement**: WASD or Arrow Keys
- **Confirm**: Enter or Space
- **Inventory**: I key
- **Cancel**: Escape or Backspace

### UI Elements
- **Health bars**: Pixel-style rectangles with fill
- **Dialogue box**: Bottom 1/3 of screen, animated text reveal
- **Menu**: Centered overlay with list navigation

## Technical Implementation

### Class Structure
```
Game (main controller)
├── Map (tile grid, collision)
├── Player (position, movement, stats)
├── EncounterSystem (random battles)
├── BattleSystem (combat logic)
├── UIManager (dialogue, menus)
└── Inventory (captured creatures)
```

### Game Loop
- `requestAnimationFrame` based
- `update(deltaTime)` - logic
- `render(ctx)` - drawing

## Acceptance Criteria

1. ✅ Single HTML file, no external dependencies
2. ✅ Player can move on map with collision
3. ✅ Random encounters in grass tiles
4. ✅ Turn-based battle with attack/capture/run
5. ✅ Creatures can be captured and stored
6. ✅ Inventory accessible via 'I' key
7. ✅ Auto-save to localStorage
8. ✅ Retro Game Boy color palette
9. ✅ Responsive canvas scaling
10. ✅ Professional code structure with classes
