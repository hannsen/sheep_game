# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Black Sheep Meadow Runner is a single-file 3D browser game built with Three.js. The player controls a black sheep in a pastoral meadow environment, with support for both desktop keyboard controls and mobile touch/joystick controls.

## Architecture

### Single-File Structure
The entire game is contained in `index.html` - there are no separate JavaScript files. The file contains:
- Inline CSS styles (lines 7-135)
- Three.js CDN import (line 159)
- All game logic in inline JavaScript (lines 160-1170)

### Key Game Systems

**Sheep Class (lines 209-361)**
- Constructor creates the player sheep mesh with body, head, eyes, pupils, legs, and ears
- `update()` method handles keyboard/touch input, movement, rotation, jumping, gravity, and collision detection
- `animateLegs()` animates leg rotation based on movement speed

**Collision System (lines 594-694)**
- `collisionObjects` array stores all collidable objects (fences, farmhouse, trees, hay bales)
- Supports two collision shapes: `circle` (for trees/hay bales) and `rectangle` (for fences/farmhouse)
- `checkCollision()` function performs distance checks and returns collision data

**White Sheep NPCs (lines 727-823)**
- 15 autonomous white sheep with grazing behavior
- Each has `userData` containing state: grazeTime, moveTimer, isMoving, targetPosition, velocity, lastBleatTime
- Head animation for grazing (lines 1095-1102)
- Movement logic with collision avoidance (lines 1042-1093)
- Collision/pushing physics with player (lines 1106-1145)

**Mobile Touch Controls (lines 883-1017)**
- Joystick-based movement (lines 886-987) with visual knob feedback
- Touch action buttons for jump and sprint (lines 989-1016)
- Automatically detected via user agent or screen width (lines 161-170)

**Environment Objects**
- Ground plane with terrain variation (lines 363-378)
- Grass blades (lines 381-397)
- Flowers (lines 400-427)
- Trees positioned around perimeter (lines 430-457)
- Fence perimeter (lines 460-515)
- Farmhouse (lines 518-563)
- Hay bales (lines 566-592)
- Clouds with animation (lines 697-721, 1160-1165)

**Audio System (lines 826-872)**
- Web Audio API for synthesized sound effects
- `playJumpSound()` creates upward pitch sweep (lines 830-848)
- `playBleatSound()` creates wavering sawtooth tone for sheep bleats (lines 851-872)

**Camera System (lines 1147-1157)**
- Third-person camera with smooth lag following the player
- Rotates around sheep with interpolation
- Offset: 15 units back, 8 units up

### Physics Constants
- Base movement speed: 35 units/sec
- Sprint multiplier: 1.8x
- Jump velocity: 6 units/sec
- Gravity: 20 units/sec²
- Friction multiplier: 0.9
- Rotation speed: 3 rad/sec

## Development Workflow

### Running the Game
Open `index.html` directly in a web browser. No build process or dev server required.

### Testing
- Desktop: Use WASD/Arrow keys to move, Shift to sprint, Space to jump
- Mobile: Use on-screen joystick and buttons (automatically shown on mobile devices)

### Making Changes
Since this is a single-file project, all edits are made directly to `index.html`. The JavaScript code is not minified or obfuscated, making it straightforward to modify.

### Key Areas for Common Modifications

**Adding new environment objects:**
1. Create geometry/material/mesh in a new function
2. Add to scene with `scene.add()`
3. If collidable, add to `collisionObjects` array with appropriate shape type

**Modifying player mechanics:**
- Movement/physics: Edit `Sheep.update()` method
- Appearance: Edit `Sheep` constructor
- Controls: Modify key event handlers (lines 874-881) or touch handlers (lines 883-1017)

**Adjusting white sheep behavior:**
- Movement patterns: Edit lines 1042-1093
- Grazing animation: Edit lines 1095-1102
- Collision response: Edit lines 1106-1145

**Camera adjustments:**
- Follow behavior: Edit lines 1147-1157
- Initial position: Edit lines 178-184

## Git Commit Patterns
Recent commits follow the pattern: verb + description (e.g., "Add mobile touch controls", "Fix grazing animation direction")
