# Web Strategy Games

> Two players, one screen. Every board is a graph.

A browser-based collection of three two-player strategy games — **Quoridor**, **Classic Hex** and **Special Hex** — built as a Discrete Mathematics project. Play against a friend on the same screen or against a built-in computer opponent.

The whole app is a **single HTML file** with no frameworks, no build step and no dependencies.

**Live demo:** `https://krishacpc1905.github.io/dm_project/index.html` 

---

## Games

| Mode | Board | Goal |
|------|-------|------|
| **Quoridor** | 9×9 squares | Reach the opposite row of the board. Use walls to slow your opponent down. |
| **Classic Hex** | 11×11 hexagonal grid | Build an unbroken chain of stones between your two edges. |
| **Special Hex** | 11×11 hexagonal grid | Classic Hex plus one-time tactical abilities and a 5:00 chess clock. |

### Quoridor

- Red starts at the bottom and races to the top row. Blue starts at the top and races to the bottom. Red moves first.
- On your turn, either **move your pawn** one square (valid moves are highlighted) or **place a wall** (2 squares long). Each player has **10 walls**.
- Pawns can jump over an adjacent opponent, or step diagonally if a wall is behind them.
- Walls cannot overlap, and a wall is **illegal if it would cut off every path** to the goal for either player. A red ghost wall previews illegal placements.

### Classic Hex

- **Red** connects **left to right**. **Blue** connects **top to bottom**. Red moves first.
- Players alternate placing one stone on any empty cell. Cells connect only along shared sides (six neighbours per cell).
- When a chain is complete, the winning path is animated across the board.

### Special Hex

Classic Hex rules, plus each player gets **one use of each ability** per game. Using an ability takes your turn.

- **Block:** make an empty cell unplayable for 2 turns.
- **Remove:** delete an enemy stone (unless it is shielded).
- **Shield:** protect one of your own stones from removal.
- Each player has a **5:00 clock**. Running out of time loses the game.

### Playing against the computer

On the main menu, set **Opponent** to **Computer**, then choose whether you play **Red (first)** or **Blue**. The computer replies shortly after each of your moves, and *Restart* keeps your choice. If the computer ever hits an error, the game falls back to manual play for both sides instead of freezing.

---

## Getting started

No installation needed.

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```

Then open `web-strategy-games-vs-computer.html` in any modern browser (Chrome, Edge, Firefox, Safari).

Prefer a local server?

```bash
python -m http.server 8000
# open http://localhost:8000
```

### Deploying with GitHub Pages

1. Rename `web-strategy-games-vs-computer.html` to `index.html` (so Pages serves it at the site root).
2. In your repository go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select your main branch and the `/ (root)` folder, then save.
4. After a minute or so the game is live at `https://<your-username>.github.io/<repo-name>/`.

---

## The discrete mathematics behind it

Each board is modelled as a **graph**: cells are vertices and shared sides are edges.

| Concept | Where it appears |
|---------|------------------|
| **Graph modelling** | Hex cells are vertices with up to 6 neighbours. Quoridor squares form a 9×9 grid graph, and placing a wall removes edges. |
| **Breadth-first search (BFS)** | Detects a completed Hex connection between a player's two edges (and recovers the winning path for the animation). |
| **Reachability / connectivity** | Validates every Quoridor wall: a wall is accepted only if both pawns still have a path to their goal row. |
| **Shortest paths** | The Quoridor computer uses a BFS distance field from the goal row. The Hex computer uses a 0–1 weighted shortest-path search (own stones cost 0, empty cells cost 1, enemy or blocked cells are impassable). |

### How the computer plays

- **Quoridor:** if the opponent is ahead in the race, it looks for the wall that most increases the gap between the two shortest paths; otherwise it steps along its own shortest path.
- **Classic Hex:** it scores each empty cell by how much it shortens its own connection, how much it lengthens yours, and a small preference for central cells, with a little randomness.
- **Special Hex:** it also uses Block, Remove and Shield situationally, more often when you are close to connecting.

This is a heuristic opponent, not a minimax or Monte Carlo search, so it is beatable.

---

## Tech

- **HTML, CSS and vanilla JavaScript** in one file
- **SVG** for the hexagonal board, **CSS Grid** for the Quoridor board, **Canvas** for the animated background graph
- Responsive layout for desktop and mobile
- Respects `prefers-reduced-motion`

## Project structure

```
.
├── web-strategy-games-vs-computer.html   # the entire game (markup, styles, logic)
└── README.md
```

## Possible improvements

- Stronger computer opponent (minimax with alpha-beta pruning, or Monte Carlo tree search)
- Online multiplayer
- Hex swap (pie) rule to balance the first-move advantage
- Move history and undo

## Author

Built by Krish as a Discrete Mathematics course project at IIT Jodhpur.

## License

Add a license of your choice (for example [MIT](https://choosealicense.com/licenses/mit/)) by creating a `LICENSE` file in the repository root.
