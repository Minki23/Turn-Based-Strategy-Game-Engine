# Turn-Based Strategy Game Engine

A fully featured, two-player local multiplayer strategy game built in Java with a Swing-based graphical user interface. The project demonstrates object-oriented design, polymorphic inheritance, and rule-enforcement algorithms operating on an 8×8 grid state machine.

## Features

- **Local multiplayer** – two human players share the same session and take alternating turns
- **Complete rule-enforcement engine** – every move is validated against a full set of game rules before it is accepted
- **Special move algorithms** – castling (both kingside and queenside), en passant capture, and pawn promotion with an interactive dialog
- **Check / checkmate / stalemate detection** – a real-time attack-surface tracker marks every square threatened by each side; the game immediately detects terminal states and notifies the players
- **Legal move generation** – each piece type computes its own reachable squares using directional ray-casting (sliding pieces) or fixed offset tables (leaping pieces), then filters out any move that would leave the player's own king in check
- **Swing GUI** – 64-button interactive board with click-to-select / click-to-move interaction, move highlighting, and piece sprites

## Architecture

The project follows an object-oriented architecture with a clear separation of concerns:

```
Field  (JButton – board square)
└── Piece  (abstract – shared game logic)
    ├── King   ← castling logic, 8-direction adjacency
    ├── Queen  ← combined ray-casting (8 directions)
    ├── Rook   ← orthogonal ray-casting
    ├── Bishop ← diagonal ray-casting
    ├── Knight ← fixed L-shaped offset table
    └── Pawn   ← forward movement, diagonal capture, en passant, promotion
```

`Main` acts as the central game controller, holding the 8×8 `Field[][]` board, two piece lists, king references, and the current-turn flag.  
Each move goes through the following pipeline:

1. Player selects a piece → available destinations are highlighted
2. Player selects a destination → legality is re-confirmed
3. `movePiece()` updates board state, handles special moves, and switches the turn
4. `attacks()` recomputes the full attack surface and evaluates check/checkmate/stalemate

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Java |
| GUI | javax.swing / java.awt |
| Build | IntelliJ IDEA project (`.iml`) |
| Dependencies | None (Java standard library only) |

## Getting Started

### Prerequisites

- Java Development Kit (JDK) 8 or higher

### Run

1. Clone the repository
2. Open the project in IntelliJ IDEA (or compile manually)
3. Run `src/Main.java`

The game window opens immediately. White moves first. Click a piece to see its legal moves highlighted in purple, then click a destination square to execute the move.

## Algorithms & Data Structures

| Concept | Implementation |
|---------|---------------|
| Board representation | `Field[8][8]` two-dimensional array |
| Piece list | `ArrayList<Piece>` per side (supports O(1) removal on capture) |
| Attack surface | Per-square `ArrayList<Piece>` tracking all attackers; rebuilt after every move |
| Ray-casting | Iterative directional loops (up to 7 squares) for sliding pieces |
| Self-check filter | `wouldPutInCheck()` simulates each candidate move on a temporary board state |
| Terminal state detection | Cross-references attack surface with king position and legal-move count |

## License

This project is open source. Feel free to fork and extend it.
