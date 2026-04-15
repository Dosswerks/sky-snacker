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

- Completing enough deliveries advances to the next level
- Each level increases the frequency of snack requests
- Patience timers get shorter
- More simultaneous requests appear
- Trash is thrown back sooner

---

## Technical Details

### Architecture

Sky Snacker is a single-file HTML5 Canvas game with no external dependencies. All game logic, rendering, audio management, and input handling are contained in one `index.html` file. It runs in any modern browser on desktop or mobile.

### Development Process

The game was developed through conversational AI-assisted coding using Kiro, following the same iterative pattern established across three prior games (Barrel Bears, Can't Drive 55, and Runway Rush).

The core concept — Tapper on an airplane — required adapting the classic bar-service mechanic to a 2D cabin layout with:
- A central aisle instead of multiple bars
- Bidirectional serving (left and right) instead of one direction
- Multiple snack types requiring selection before serving
- Thought bubble patience system with visual urgency indicators
- Trash return mechanic adapted from Tapper's empty glass returns

### Key Technical Features

- **Thought bubble system**: Each passenger has an independent patience timer with visual color progression and urgency pulse animation
- **Arc trajectory**: Flung snacks follow a sine-curve arc over passenger heads to reach the target seat
- **Auto-targeting**: Snacks automatically find the requesting passenger in the served row and side
- **Trash physics**: Trash items slide from the served seat back toward the aisle at variable speeds
- **Mobile controls**: Touch button panel appears automatically on mobile devices
- **Sprite system**: 3-frame Bridget animation (center, left-fling, right-fling)

### Asset System

All custom assets are defined in the config object:

```javascript
const A = {
    boxArtImage: 'assets/box-art.png',       // splash screen
    cabinImage: null,                         // 480x600 cabin background
    bridgetCenter: null,                      // 60x80 default pose
    bridgetLeft: null,                        // 60x80 flinging left
    bridgetRight: null,                       // 60x80 flinging right
    seatImage: null,                          // 60x40 seat back
    snack0Image: null,                        // 24x24 pretzel
    snack1Image: null,                        // 24x24 cookie
    snack2Image: null,                        // 24x24 soda
    trashImage: null,                         // 20x20 trash
    serveSound: null,                         // fling sound
    crashSound: null,                         // collision/error
    catchSound: null,                         // catch trash or deliver
    angrySound: null,                         // patience expired
    wrongSound: null,                         // wrong snack
    levelSound: null,                         // level complete
    backgroundMusic: null,                    // loops continuously
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
| Trash | 20 × 20 px | PNG w/ transparency | Thrown-back trash item |

### Sound Specs

| Asset | Duration | Format | Notes |
|---|---|---|---|
| serve | 0.2–0.4 sec | MP3 | Snack flung |
| catch | 0.2–0.3 sec | MP3 | Trash caught or correct delivery |
| angry | 0.3–0.5 sec | MP3 | Patience expired |
| wrong | 0.2–0.4 sec | MP3 | Wrong snack served |
| crash | 0.3–0.5 sec | MP3 | General error |
| level | 0.5–1.0 sec | MP3 | Level complete |
| music | 30–120 sec | MP3 | Background music, loops |

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
    snack0.png (pretzel)
    snack1.png (cookie)
    snack2.png (soda)
    trash.png
    serve.mp3
    catch.mp3
    angry.mp3
    wrong.mp3
    crash.mp3
    level.mp3
    music.mp3
```

---

## Credits

Concept, design, art direction, and audio: [Andrew Doss](https://www.andrewdoss.com/)

Game engine and code: Built with Kiro AI-assisted development

Hosted on GitHub Pages
