### link: https://tictactoeszadolowski.netlify.app

# React Tic-Tac-Toe

## Description

This is a simple implementation of the classic Tic-Tac-Toe game using React. The application allows two players to play against each other, with the game state managed through React hooks and components. The game board visually displays the current game state, and it includes options for restarting the game and editing player names.

### Features:

- Two-player Tic-Tac-Toe game
- Turn-based game flow
- Editable player names
- Game-over screen with a rematch option
- Simple, responsive UI

---

## Getting Started

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/tic-tac-toe.git
   ```
   Navigate to the project directory:
   ```bash
   cd tic-tac-toe
   ```
   Install the dependencies:
   ```bash
   npm install
   ```

### Running the Application

Start the development server:

```bash
npm run dev
```

#### Open your browser and navigate to <code>http://localhost:3000</code> to view the app.

File Structure

```plaintext
├── App.jsx
├── assets
│ └── react.svg
├── components
│ ├── GameBoard.jsx
│ ├── GameOver.jsx
│ ├── Log.jsx
│ └── Player.jsx
├── index.css
├── index.jsx
└── winning-combinations.js
```

## Key Components

### <code>GameBoard.jsx</code><br>

This component renders the game board. It maps the 3x3 grid of the Tic-Tac-Toe board, allowing the player to click and select a square.

```javascript

export default function GameBoard({ onSelectSquare, board }) {
return (

<ol id="game-board">
{board.map((row, rowIndex) => (
<li key={rowIndex}>
<ol>
{row.map((playerSymbol, colIndex) => (
<li key={colIndex}>
<button
onClick={() => onSelectSquare(rowIndex, colIndex)}
disabled={playerSymbol !== null} >
{playerSymbol}
</button>
</li>
))}
</ol>
</li>
))}
</ol>
);
}
```

### <code>GameOver.jsx</code><br>

This component handles the end of the game, showing the winner or a draw and offering a rematch button.

```javascript

export default function GameOver({ winner, onRestart }) {
return (

<div id="game-over">
<h2>Game Over!</h2>
{winner && <p>{winner} won!</p>}
{!winner && <p>It's a draw!</p>}
<p>
<button onClick={onRestart}>Rematch!</button>
</p>
</div>
);
}
```

### <code>Player.jsx</code><br>

This component allows each player to edit their name and displays their turn. The player name can be changed by toggling between the edit and save buttons.

```javascript

export default function Player({ initialName, symbol, isActive, onChangeName }) {
const [playerName, setPlayerName] = useState(initialName);
const [isEditing, setIsEditing] = useState(false);

function handleEditClick() {
setIsEditing((editing) => !editing);
if (isEditing) {
onChangeName(symbol, playerName);
}
}

return (

<li className={isActive ? "active" : undefined}>
<span className="player">
{isEditing ? (
<input type="text" value={playerName} onChange={(e) => setPlayerName(e.target.value)} />
) : (
<span className="player-name">{playerName}</span>
)}
<span className="player-symbol">{symbol}</span>
</span>
<button onClick={handleEditClick}>{isEditing ? "Save" : "Edit"}</button>
</li>
);
}
```

### <code>App.jsx</code><br>

This is the main file that combines all components and manages the game state using React's <code>useState</code> hook. It calculates whose turn it is and checks for a winner or a draw.

```javascript
function App() {
const [players, setPlayers] = useState(PLAYERS);
const [gameTurns, setGameTurns] = useState([]);
const activePlayer = deriveActivePlayer(gameTurns);
const gameBoard = deriveGameBoard(gameTurns);
const winner = deriveWinner(gameBoard, players);
const hasDraw = gameTurns.length === 9 && !winner;

return (

<main>
<div id="game-container">
<ol id="players">
<Player
initialName={players.X}
symbol="X"
isActive={activePlayer === "X"}
onChangeName={(symbol, name) => setPlayers({ ...players, [symbol]: name })}
/>
<Player
initialName={players.O}
symbol="O"
isActive={activePlayer === "O"}
onChangeName={(symbol, name) => setPlayers({ ...players, [symbol]: name })}
/>
</ol>
{(winner || hasDraw) && <GameOver winner={winner} onRestart={() => setGameTurns([])} />}
<GameBoard onSelectSquare={(row, col) => setGameTurns([...gameTurns, { row, col }])} board={gameBoard} />
</div>
<Log turns={gameTurns} />
</main>
);
}
```
