# 🎲 LUDO — C++ Desktop Edition

A fully playable 2–4 player Ludo board game built in **C++17** with the [raylib](https://www.raylib.com/) graphics library — complete graphical UI, animated dice, undo/redo, save/load, and move replay.

![Language](https://img.shields.io/badge/language-C%2B%2B17-blue?logo=cplusplus)
![Graphics](https://img.shields.io/badge/graphics-raylib-orange)
![Players](https://img.shields.io/badge/players-2--4-green)
![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey)

---

## ✨ Features

- Classic Ludo rules: base-exit on 6, bonus turns, 3-sixes forfeit, captures, safe squares, home column
- Click-to-roll / click-to-move with highlighted legal moves
- 3 board themes (Classic, Dark, Pastel) and custom player names
- Undo / redo (stack-based move history)
- Save / load to a plain-text file
- Full move-by-move replay of a finished game

---

## 🕹️ How to Play

1. Launch → **Main Menu** (New Game / Load Game / Quit)
2. Pick 2–4 players, enter names, choose a theme, press **START**
3. Each turn: roll the dice, then click a highlighted token to move it
4. Rolling a 6 lets you leave base and grants an extra turn; three 6's in a row forfeits the turn
5. Landing on an opponent on a non-safe square sends it back to base
6. First player to get all 4 tokens to the center wins

---

## ⌨️ Key Controls

| Key | Action |
|---|---|
| `SPACE` / click dice | Roll |
| Click token | Move |
| `U` / `R` | Undo / Redo |
| `S` / `L` | Save / Load |
| `P` | Replay mode |
| `←` `→` | Step replay back/forward |
| `ESC` | Back / quit |

---

## 🏗️ Architecture

Layered, MVC-style design:

- **`Renderer`** — raylib drawing + input, calls into `Game`
- **`Game`** — turn flow, dice, win detection, wires everything together
- **`Board`** — movement rules, validity checks, captures (owns 52 `Cell`s)
- **`Player` / `Token` / `Position`** — core data entities
- **`Move` / `MoveManager`** — command-pattern undo/redo + replay history
- **`StateManager`** — save/load game state to text file

Notable techniques: `enum class` state machines, `std::stack` for undo/redo, `std::set` for O(log n) safe-square lookup, modular arithmetic for track wraparound, operator overloading, and raw-pointer aggregation (`Cell` holds `Token*`, doesn't own it).

---

## 🗂️ File Structure

```
LUDO_FIXED/
├── main.cpp
├── Game.h / Game.cpp          # turn orchestration, dice, win check
├── Board.h / Board.cpp        # track, movement, capture rules
├── Cell.h / Cell.cpp          # a single track square
├── Player.h / Player.cpp      # player + 4 tokens
├── Token.h / Token.cpp        # a single piece
├── Position.h                 # "where on the board" value type
├── Move.h / Move.cpp          # immutable move record
├── MoveManager.h / .cpp       # undo/redo stacks + history
├── StateManager.h / .cpp      # save/load
└── Renderer.h / Renderer.cpp  # raylib drawing + input
```

---

## ⚙️ Build & Run

Requires **raylib** + a C++17 compiler.

```bash
# Install raylib
sudo apt install libraylib-dev      # Linux
brew install raylib                 # macOS
vcpkg install raylib                # Windows

# Compile (Linux/macOS)
g++ -std=c++17 *.cpp -o ludo -lraylib -lGL -lm -lpthread -ldl -lrt -lX11

# Compile (Windows/MinGW)
g++ -std=c++17 *.cpp -o ludo.exe -lraylib -lopengl32 -lgdi32 -lwinmm

# Run
./ludo        # Linux/macOS
ludo.exe      # Windows
```

---

## 💾 Save Format

`S` writes to `ludo_save.txt` (format `LUDO_SAVE_V2`): player states, token positions, and full move history, enough to reconstruct undo/redo/replay on load. Older `LUDO_SAVE_V1` files still load.

---

## 🚀 Future Ideas

- AI opponents
- Online multiplayer
- Sound effects
- Responsive window sizing
- Unit tests for movement/capture logic

---

<div align="center">Made with ❤️, C++17, and raylib.</div>
