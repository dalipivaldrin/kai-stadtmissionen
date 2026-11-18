# Kai – City Missions 🏙️

A 2D browser platformer built with plain **HTML5 Canvas and JavaScript** – no framework, no image files, no external dependencies. Everything (character, enemies, backgrounds, sounds) is generated directly in code.

**▶ Play now:** https://dalipivaldrin.github.io/kai-stadtmissionen/

> The in-game text is in German.

---

## The Idea

I wanted to make a platformer in the style of classic jump'n'run games – but with **my own character and my own game concept**, not a copy of Super Mario.

The result is **Kai**, a city courier with a teal hoodie, headphones and a backpack. Instead of just running to the end of the level, Kai has to complete a **mission** in every level. The goal only unlocks once the mission is done.

| Mission | Theme | Task | Goal |
|---|---|---|---|
| 1 – The Courier | City by day | Collect 5 scattered packages | Mailbox |
| 2 – Blackout | Factory at dusk | Flip 5 emergency power switches | Factory door |
| 3 – Kitten Rescue | Rooftops at night | Rescue 5 kittens (they follow Kai) | Animal shelter |

## Controls

| Action | Keyboard | Mobile |
|---|---|---|
| Move | ← → or A / D | ◀ ▶ |
| Jump | ↑, W or Space | ▲ |
| Pause | P or Esc | – |
| Confirm | Enter / Space | Tap the screen |

**Tips:** Jump on robots and drones to defeat them. Holding jump while stomping gives a higher bounce. Checkpoint flags save your progress.

---

## How I Built It

### 1. Defining the concept
Before writing any code, I decided on:
- **Genre:** side-scrolling platformer with missions
- **Main character:** an original character (Kai), clearly different from well-known game characters
- **Scope:** 3 levels with increasing difficulty, each with its own mission and color theme
- **Tech:** a single `index.html` so the game runs anywhere without installation

### 2. Technical foundation
The game is built on a classic **game loop**:
- `update()` handles movement, collisions and game logic
- `draw()` renders everything onto the canvas
- A **fixed time step** (60 updates per second) makes the game run at the same speed on fast and slow screens

The game uses **states** (`title`, `intro`, `play`, `pause`, `clear`, `gameover`, `win`) and switches between them.

### 3. Levels as a tilemap
The world is a grid of 32×32-pixel tiles (16 rows high). Instead of typing each level character by character, I wrote small **builder functions**:

```js
L.ground(0, 25);     // ground from column 0 to 25
L.plat(11, 12, 16);  // crate platform in row 11
L.put('M', 10, 14);  // place a mission item
L.put('G', 13, 22);  // place a ground robot
```

This makes levels quick to adjust. When designing levels, I made sure platforms are at most 3 tiles apart vertically and gaps are at most 4 tiles wide – matching Kai's maximum jump height and distance.

### 4. Physics & controls
To make jumping feel good, I used a few techniques from professional platformers:
- **Acceleration & friction** instead of instant speed
- **Variable jump height:** tap = small jump, hold = high jump
- **Coyote time:** you can still jump for a moment after running off a ledge
- **Jump buffer:** a jump pressed just before landing still triggers

Collision is resolved separately on the **X and Y axes** against the tiles (AABB collision).

### 5. Enemies
- **Scrap bot:** drives back and forth on the ground and turns around at walls, ledges and spikes
- **Drone:** flies back and forth in a wave pattern

If Kai lands on an enemy from above, it is defeated. Touching it from the side costs a life and makes Kai briefly invulnerable.

### 6. Graphics without image files
All graphics are drawn with canvas shapes: Kai, robots, drones, packages, switches, kittens, buildings. The backgrounds use two **parallax layers** (distant buildings move slower), generated with a seeded random number generator so each level always looks the same.

### 7. Sound
Sound effects (jump, coin, hit, mission complete …) are generated live with the **Web Audio API** – short tones from oscillators, no audio files.

### 8. Missions, score & progress
- Mission progress is shown in the top bar (e.g. `Pakete: 3 / 5`)
- The goal is locked until the mission is complete
- Points come from coins, enemies, mission items, a time bonus and remaining lives
- The high score is saved in the browser (`localStorage`)

### 9. Mobile support
On touch devices, on-screen control buttons appear automatically. The canvas scales to any screen size.

### 10. Testing
I tested the game automatically with **Playwright** (headless browser): all three levels load, controls respond, every level can be completed and no JavaScript errors occur. After that came fine-tuning by playing it myself.

---

## Project Structure

```
kai-stadtmissionen/
├── index.html      # The complete game (HTML, CSS, JavaScript)
└── README.md       # This documentation
```

## Run Locally

Download the repository and open `index.html` in your browser – that's it. No server, no installation needed.

```bash
git clone https://github.com/dalipivaldrin/kai-stadtmissionen.git
cd kai-stadtmissionen
# open index.html in your browser
```

## Ideas for Later
- More missions and levels
- Moving platforms and power-ups
- High score leaderboard
- Level editor
- English language option in the game

---

Developed by **Valdrin Dalipi** – with AI assistance (Claude) for planning and programming.
