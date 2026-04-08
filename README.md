# heres the code so you can fanmade



```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Maze Game (Optimized)</title>
  <meta name="description" content="Play a fun maze game online with adjustable difficulty">
  <style>
    body {
      margin: 0;
      background: #0b1a1f;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      font-family: Arial, sans-serif;
      color: white;
    }

    #controls {
      margin-bottom: 10px;
    }

    select, button {
      padding: 5px 10px;
      margin-right: 10px;
      font-size: 14px;
    }

    canvas {
      background: #111;
      border: 2px solid #00e5ff;
    }
  </style>
</head>
<body>

<div id="controls">
  <label>Difficulty:</label>
  <select id="difficulty">
    <option value="10">Easy</option>
    <option value="15">Medium</option>
    <option value="20">Hard</option>
  </select>
  <button onclick="startGame()">Start</button>
</div>

<canvas id="mazeCanvas" width="400" height="400"></canvas>

<script>
const canvas = document.getElementById("mazeCanvas");
const ctx = canvas.getContext("2d");

let rows = 10;
let cols = 10;
let cellSize = 40;

let maze, player, goal, gameOver;

function initGame(size) {
  rows = cols = size;
  cellSize = canvas.width / cols;

  maze = [];
  player = { x: 0, y: 0 };
  goal = { x: cols - 1, y: rows - 1 };
  gameOver = false;

  for (let y = 0; y < rows; y++) {
    let row = [];
    for (let x = 0; x < cols; x++) {
      row.push({
        visited: false,
        walls: { top: true, right: true, bottom: true, left: true }
      });
    }
    maze.push(row);
  }

  generateMaze();
  drawMaze();
}

function generateMaze() {
  let stack = [[0, 0]];
  maze[0][0].visited = true;

  while (stack.length > 0) {
    let [x, y] = stack[stack.length - 1];

    let neighbors = [];
    if (y > 0 && !maze[y-1][x].visited) neighbors.push([0, -1, "top", "bottom"]);
    if (x < cols-1 && !maze[y][x+1].visited) neighbors.push([1, 0, "right", "left"]);
    if (y < rows-1 && !maze[y+1][x].visited) neighbors.push([0, 1, "bottom", "top"]);
    if (x > 0 && !maze[y][x-1].visited) neighbors.push([-1, 0, "left", "right"]);

    if (neighbors.length > 0) {
      let [dx, dy, wall, opposite] = neighbors[Math.floor(Math.random() * neighbors.length)];
      let nx = x + dx;
      let ny = y + dy;

      maze[y][x].walls[wall] = false;
      maze[ny][nx].walls[opposite] = false;

      maze[ny][nx].visited = true;
      stack.push([nx, ny]);
    } else {
      stack.pop();
    }
  }
}

function drawMaze() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  for (let y = 0; y < rows; y++) {
    for (let x = 0; x < cols; x++) {
      let cell = maze[y][x];
      let px = x * cellSize;
      let py = y * cellSize;

      ctx.strokeStyle = "#00e5ff";
      ctx.lineWidth = 1;

      if (cell.walls.top) drawLine(px, py, px + cellSize, py);
      if (cell.walls.right) drawLine(px + cellSize, py, px + cellSize, py + cellSize);
      if (cell.walls.bottom) drawLine(px, py + cellSize, px + cellSize, py + cellSize);
      if (cell.walls.left) drawLine(px, py, px, py + cellSize);
    }
  }

  ctx.fillStyle = "#00ff88";
  ctx.fillRect(goal.x * cellSize + 4, goal.y * cellSize + 4, cellSize - 8, cellSize - 8);

  ctx.fillStyle = "#ff3b3b";
  ctx.beginPath();
  ctx.arc(player.x * cellSize + cellSize/2, player.y * cellSize + cellSize/2, cellSize/4, 0, Math.PI * 2);
  ctx.fill();
}

function drawLine(x1, y1, x2, y2) {
  ctx.beginPath();
  ctx.moveTo(x1, y1);
  ctx.lineTo(x2, y2);
  ctx.stroke();
}

function movePlayer(e) {
  if (gameOver) return;

  let cell = maze[player.y][player.x];

  // Arrow keys + WASD support
  if ((e.key === "ArrowUp" || e.key.toLowerCase() === "w") && !cell.walls.top) player.y--;
  if ((e.key === "ArrowRight" || e.key.toLowerCase() === "d") && !cell.walls.right) player.x++;
  if ((e.key === "ArrowDown" || e.key.toLowerCase() === "s") && !cell.walls.bottom) player.y++;
  if ((e.key === "ArrowLeft" || e.key.toLowerCase() === "a") && !cell.walls.left) player.x--;

  if (player.x === goal.x && player.y === goal.y) {
    gameOver = true;
    showWinUI();
    return;
  }

  drawMaze();
}

  drawMaze();
}

window.addEventListener("keydown", movePlayer);

function showWinUI() {
  ctx.fillStyle = "rgba(0,0,0,0.7)";
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  ctx.fillStyle = "#00ffcc";
  ctx.font = "bold 28px Arial";
  ctx.textAlign = "center";
  ctx.fillText("🎉 YOU WIN 🎉", canvas.width / 2, canvas.height / 2);

  ctx.font = "14px Arial";
  ctx.fillStyle = "#ffffff";
  ctx.fillText("Press R to Restart", canvas.width / 2, canvas.height / 2 + 30);
}

window.addEventListener("keydown", (e) => {
  if (e.key.toLowerCase() === "r") startGame();
});

function startGame() {
  const size = parseInt(document.getElementById("difficulty").value);
  initGame(size);
}

// Start default game
startGame();
</script>
</body>
</html>
```


# name it maze etc put .html at end
