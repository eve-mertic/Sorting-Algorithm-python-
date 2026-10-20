# Sorting Algorithm Visualizer

A simple desktop app built with Python and [Pygame](https://www.pygame.org/) that animates how sorting algorithms work. Watch a randomly generated list of bars get sorted step by step, in ascending or descending order.

## Features

- Visualizes **Bubble Sort** and **Insertion Sort**
- Sort in **ascending** or **descending** order
- Highlights the bars being compared/swapped (green and red)
- Generate a fresh random list at any time
- Keyboard-driven controls, with a live on-screen legend

## Requirements

- Python 3.7+
- Pygame

## Installation

```bash
pip install pygame
```

## Usage

Save the script as `sorting_algorithms.py` and run:

```bash
python sorting_algorithms.py
```

A window opens showing 50 random bars. Pick an algorithm and order, then press **Space** to start.

## Controls

| Key | Action |
|-----|--------|
| `SPACE` | Start sorting |
| `R` | Reset with a new random list |
| `A` | Sort in ascending order |
| `D` | Sort in descending order |
| `B` | Select Bubble Sort |
| `I` | Select Insertion Sort |

Algorithm and order can only be changed while sorting is not in progress. Press `R` to stop a sort mid-run and generate a new list.

## How It Works

- **Generators for animation:** each sorting function is a Python generator that `yield`s after every swap. The main loop calls `next()` once per frame, redrawing the bars between steps. This keeps the UI responsive without threads.
- **`DrawingInformation`:** holds the window, fonts, colors, padding, and the list being sorted. It computes block width and height so the bars scale to fit the window.
- **`draw` / `draw_list`:** render the title, control hints, and the bars. During sorting, `draw_list` redraws only the bar area and highlights the active positions.
- **Colors:** bars alternate between three shades of gray for readability; the two bars being swapped are shown in green and red.

## Configuration

Adjust these values in `main()` and `DrawingInformation`:

| Setting | Location | Default | Description |
|---------|----------|---------|-------------|
| `n` | `main()` | `50` | Number of bars |
| `min_val` / `max_val` | `main()` | `0` / `100` | Range of random values |
| Window size | `DrawingInformation(800, 600, lst)` | `800x600` | Width and height in pixels |
| `clock.tick(120)` | `main()` | `120` | Max frames (steps) per second; lower it to slow the animation |
| `SIDE_PAD` / `TOP_PAD` | `DrawingInformation` | `100` / `150` | Padding around the bar area |

## Adding a New Algorithm

1. Write a generator function that takes `(draw_info, ascending=True)`, sorts `draw_info.lst` in place, and `yield True` after each visible step. Call `draw_list(draw_info, {index: color, ...}, True)` before yielding to highlight bars.
2. Add a key binding in the event loop in `main()` that sets `sorting_algorithm` and `sorting_algorithm_name`.
3. Add the new option to the on-screen legend in `draw()`.

## Known Limitations

- If every value in the list is identical (`max_val == min_val`), the block height calculation divides by zero. This is very unlikely with the default settings.
- Only two algorithms are included.

## License

MIT
