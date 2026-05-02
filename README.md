# Sky Snacker

*Serve fast. Catch the trash. Keep them happy.*

Sky Snacker is an arcade-style service game inspired by the classic Tapper. The player controls Bridget, a flight attendant on a packed airplane, serving snacks to impatient passengers and catching their trash before it hits the aisle floor.

---

## Gameplay

The screen shows the interior of an airplane cabin from behind. Five rows of seats line both sides of a central aisle, three seats per side, all occupied. Bridget stands in the aisle behind her snack cart, facing the player.

Passengers request snacks by displaying thought bubbles above their heads. Each bubble shows the snack they want and changes color as their patience runs out (white → pink → red → burst). The player moves Bridget up and down the aisle, selects the correct snack, and flings it to the requesting passenger.

After being served, passengers eventually throw their trash back into the aisle. Bridget must be in the correct row to catch it. Missed trash and expired patience both count as misses. Five misses ends the game.

## Snacks

Three snack types are available:
- 🥨 Pretzel (key 1)
- 🍪 Cookie (key 2)
- 🥤 Soda (key 3)

Flinging the wrong snack to a passenger results in a penalty.

## Controls

**Desktop:**
- Up/Down arrow keys: Move Bridget along the aisle
- Left/Right arrow keys: Fling the selected snack to that side
- 1, 2, 3: Select snack type
- R: Restart
- Q: Quit to title screen

**Mobile:**
- Snack selection buttons appear below the game
- Directional buttons for movement and serving
- Tap to start/restart

## Scoring

| Action | Points |
|---|---|
| Correct snack delivery | +100 |
| Catch trash | +25 |
| Wrong snack | -25 |
| Expired patience | -50 |
| Missed trash | -25 |

Five total misses (expired patience + missed trash) ends the game.

## Difficulty Progression

- Completing enough deliveries advances to the next level (all active orders must be cleared first)
- Request spawn interval starts at ~1.5 seconds and drops by ~0.08s per level (floor ~0.8s)
- Patience timers start at ~10 seconds on level 1 and shrink by ~0.5s per level (floor ~5s)
- Trash return delay shortens each level, giving less breathing room
- Trash slide speed starts at 1.0 and increases by 0.2 per level
- More simultaneous requests appear as spawn rate increases

## Attract Mode

After approximately 8 seconds of idle time on the title screen, an AI-driven demo begins automatically. The demo runs a silent, invincible playthrough of level 1 for about 15 seconds, then cycles back to the title screen. "CLICK OR TAP TO START" flashes over the gameplay. Any real input (key, touch, or click) immediately breaks out of the demo and returns to the title screen; a click or tap also starts the game.

---

## Technical Details

### Architecture

Sky Snacker is a single-file HTML5 Canvas game with no external dependencies (aside from a QR code library for the tip jar). All game logic, rendering, audio management, and input handling are contained in one `index.html` file. It runs in any modern browser on desktop or mobile.

### Development Process

The game was developed through conversational AI-assisted coding using Kiro, following the same iterative pattern established across prior games in the Dosswerks Arcade collection.

The core concept — Tapper on an airplane — required adapting the classic bar-service mechanic to a 2D cabin layout with:
- A central aisle instead of multiple bars
- Bidirectional serving (left and right) instead of one direction
- Multiple snack types requiring selection before serving
- Thought bubble patience system with visual urgency indicators
- Trash return mechanic adapted from Tapper's empty glass returns

### Key Technical Features

- **Level announcement**: "LEVEL X" displays for 2 seconds before each level begins, pausing all action
- **Level completion**: All active orders, trash, and flying snacks must be cleared before advancing — no new requests spawn once the serve target is met
- **Thought bubble system**: Each passenger has an independent patience timer with visual color progression and urgency pulse animation; an audio cue plays when a bubble turns red
- **Arc trajectory**: Flung snacks follow a sine-curve arc over passenger heads to reach the target seat
- **Auto-targeting**: Snacks automatically find the requesting passenger in the served row and side
- **Trash physics**: Trash items slide from the served seat back toward the aisle at variable speeds that increase per level
- **Cockpit and galley**: Drawn cockpit with fuselage curve, sky, clouds, and instrument gauge arrays for pilot/copilot stations; galley area with snack cart silhouettes and large selected-snack indicator
- **Mobile controls**: Touch button panel appears automatically on mobile devices
- **Sprite system**: 3-frame Bridget animation (center, left-fling, right-fling) with cart and wheels
- **Attract mode**: AI-driven demo cycles on the title screen after idle timeout, following the same pattern used in Mountains of Madness
- **Comprehensive sound design**: Dedicated audio cues for movement, throwing, catching trash, failures, patience warnings, and game over

### Sound Design

The game uses distinct sound effects for each player action and game event:
- **move**: Bridget moves up or down the aisle
- **throw**: Snack is flung toward a passenger
- **serve**: Snack delivery confirmation
- **trashCatch**: Trash successfully caught in the aisle
- **catch**: Correct snack delivery
- **angry**: Patience expired or missed trash
- **wrong**: Wrong snack delivered
- **patience**: Warning when a passenger's thought bubble turns red
- **gameover**: Game over sting
- **level**: Level complete fanfare
- **announce**: Level announcement
- **music**: Background music loop

All sounds are silenced during attract mode demos.

### Asset System

All custom assets are defined in the config object:

```javascript
const A = {
    boxArtImage: 'assets/box-art.png',
    cabinImage: null,
    bridgetCenter: 'assets/bridget-center.png',
    bridgetLeft: 'assets/bridget-left.png',
    bridgetRight: 'assets/bridget-right.png',
    seatImage: 'assets/seat.png',
    snack0Image: 'assets/snack-pretzel.png',
    snack1Image: 'assets/snack-cookie.png',
    snack2Image: 'assets/snack-soda.png',
    trashImage: 'assets/trash.png',
    serveSound: 'assets/serve.mp3',
    catchSound: 'assets/catch.mp3',
    angrySound: 'assets/angry.mp3',
    wrongSound: 'assets/wrong.mp3',
    levelSound: 'assets/level.mp3',
    announceSound: 'assets/announce.mp3',
    levelUpSound: 'assets/levelup.mp3',
    moveSound: 'assets/move.mp3',
    throwSound: 'assets/throw.mp3',
    trashCatchSound: 'assets/trash-catch.mp3',
    failSound: null,
    patienceSound: 'assets/patience.mp3',
    gameOverSound: 'assets/gameover.mp3',
    backgroundMusic: 'assets/music.mp3',
};
```

### Image Specs

| Asset | Dimensions | Format | Notes |
|---|---|---|---|
| Box Art | Any | PNG/JPG | Splash screen |
| Cabin | 480 × 600 px | PNG/JPG | Full cabin interior background |
| Bridget Center | 60 × 80 px | PNG w/ transparency | Default pose, facing player |
| Bridget Left | 60 × 80 px | PNG w/ transparency | Flinging left |
| Bridget Right | 60 × 80 px | PNG w/ transparency | Flinging right |
| Seat | 60 × 40 px | PNG w/ transparency | Seat back |
| Pretzel | 24 × 24 px | PNG w/ transparency | Snack type 0 |
| Cookie | 24 × 24 px | PNG w/ transparency | Snack type 1 |
| Soda | 24 × 24 px | PNG w/ transparency | Snack type 2 |
| Trash | 20 × 20 px | PNG w/ transparency | Drawn at 75% size (15 × 15 px on canvas) |

### Sound Specs

| Asset | File | Duration | Format | Notes |
|---|---|---|---|---|
| serve | serve.mp3 | 0.2–0.4 sec | MP3 | Snack flung confirmation |
| catch | catch.mp3 | 0.2–0.3 sec | MP3 | Correct delivery |
| angry | angry.mp3 | 0.3–0.5 sec | MP3 | Patience expired or missed trash |
| wrong | wrong.mp3 | 0.2–0.4 sec | MP3 | Wrong snack served |
| level | level.mp3 | 0.5–1.0 sec | MP3 | Level complete |
| announce | announce.mp3 | 0.5–1.0 sec | MP3 | Level announcement |
| levelup | levelup.mp3 | 1.0–2.0 sec | MP3 | Level up fanfare with announcement |
| move | move.mp3 | 0.1–0.2 sec | MP3 | Player movement up/down |
| throw | throw.mp3 | 0.2–0.3 sec | MP3 | Snack throw/fling |
| trash-catch | trash-catch.mp3 | 0.2–0.3 sec | MP3 | Trash caught in aisle |
| patience | patience.mp3 | 0.3–0.5 sec | MP3 | Warning when bubble turns red |
| gameover | gameover.mp3 | 1.0–2.0 sec | MP3 | Game over sting |
| music | music.mp3 | 30–120 sec | MP3 | Background music, loops |

### File Structure

```
sky-snacker/
  index.html
  story.html
  README.md
  assets/
    box-art.png
    cabin.png
    bridget-center.png
    bridget-left.png
    bridget-right.png
    seat.png
    snack-pretzel.png
    snack-cookie.png
    snack-soda.png
    trash.png
    serve.mp3
    catch.mp3
    angry.mp3
    wrong.mp3
    level.mp3
    announce.mp3
    levelup.mp3
    move.mp3
    throw.mp3
    trash-catch.mp3
    patience.mp3
    gameover.mp3
    music.mp3
```

---

## Credits

Concept, design, art direction, and audio: [Andrew Doss](https://www.andrewdoss.com/)

Game engine and code: Built with Kiro AI-assisted development

Hosted on GitHub Pages
