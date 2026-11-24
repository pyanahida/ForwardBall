# Game

Game Name: Forward Ball

> Repo text: Control your ball and move it forward!

# Basic Gameplay

The player controls a ball and races on a 2D plane.

# Map

The map is a square grid with width 5 and height n. From bottom to top, y increases; from left to right, x increases.

The bottom row is y=1, and the leftmost column is x=1.

After pressing the start button, the ball automatically moves forward. The player swipes left and right on the screen to move the ball left and right, thereby avoiding voids and obstacles. The ball's radius is $\frac{1}{2}$.

The ball's movement speed depends on the "ball speed" $v$, with `y+=v` per second.

During rendering, the map moves downward, and the ball only moves left and right. 30FPS is sufficient.

# Cells

Each cell can store one Floor and one Barrier.

## Floor

The detection area of a Floor is the square occupied by that cell.

1. Void
    - Nothing is placed.
    - Once 70% or more of the ball overlaps with the union of all voids on the field, the game ends.
    - Void is the only category that prevents the cell from having a Barrier.
    - Void is the only category among all Floors that does not render borders and fill colors by default.
2. Land
    - The basic road, nothing special.
    - The fill color uses `scene["land"]`, and the border color uses `scene["border"]`.
3. Jump
    - As long as the ball overlaps with it by 10%, the ball's `fly` is set to `true` for 4 cells.
    - While `fly` is true, the ball ignores game-over caused by void.
    - The fill color uses `scene["jump"]`, and the border color uses `scene["border"]`.

## Barriers

### Categories

Divided into 4 categories:

| Category | Has "effect" on `fly` ball | Has "effect" on `!fly` ball |
| --- | --- | --- |
| A | No | Yes |
| B | Yes | No |
| C | Yes | Yes |
| D | No | No |

### Detection Area

Each barrier has `x`, `y`, `type`, `dx`, and `dy` attributes. `type` is `A-D`. `x` and `y` are the barrier position. So what are `dx` and `dy` for?

---

When not given in the document, the default values of `dx` and `dy` are both `0`.

All cells within $x \in [x, x+dx]$ and $y \in [y, y+dy]$ are called the Hindarea.

Except that $(x, y)$ must be on a non-Void cell, the rest may be on anything. When rendering the icon, it is only rendered on $(x, y)$.

---

The detection area of a Barrier is the small square formed by connecting the midpoints of the four sides of the squares occupied by the Hindarea in order. This square is called the "Bararea".

Once 20% or more of the ball overlaps with the union of all "Bararea"s, the game ends. This kind of game-over overlap is called an "effect".

---

1. Bollard
    - Category: A
2. JackUp
    - Category: B
3. JackDowm
    - Category: A
4. HammerLeft
    - Category: A
    - dx: -2
5. HammerRight
    - Category: A
    - dx: 2
6. BarLeft
    - Category: A
    - dx: -2
7. BarRight
    - Category: A
    - dx: 2
8. PendulumLeft
    - Category: A
    - dx: -1
9. PendulumRight
    - Category: A
    - dx: 1
10. Wheel
    - Category: A
11. Hexagram
    - Category: B
12. Cannon
    - Category: A
    - dy: -5
13. Cannon2
    - Category: A
    - dy: -5

---

Textures are located at `./assets/texture/barrier/<corrected name>.png`, and all sizes are 256*256px.

Name correction rules:
1. The original name is the camelCase name in the text above.
2. Step 1: Change the first uppercase letter to lowercase.
3. Step 2: Change all subsequent uppercase letters to lowercase and insert an underscore before them. Numbers remain unchanged.
4. For example: `TestFoo` → `test_foo`

# Scenes

## Switching Scenes

Through the specified configuration, the ball can switch scenes when `y=specified value`.

The configuration can also record the initial scene.

## Scene Object

The name of each scene conforms to PEP8 variable naming conventions. The structure is as follows:

```json
{
    "land": "#RRGGBB",
    "border": "#RRGGBB",
    "jump": "#RRGGBB",
    "glass": "#RRGGBBAA",
    "glass_border": "#RRGGBB",
    "glass_border_width": number,
    "background": "#RRGGBB"
}
