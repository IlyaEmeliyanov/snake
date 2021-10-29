<!-- Scripts -->
<script>
  import Score from "./Score.svelte";
  import Timer from "./Timer.svelte";
  import Cell from "./Cell.svelte";

  var rows = 15;
  var cols = 15;
  var coords = { x: 0, y: 0 };
  var speed = 100; // ms per block
  var key = "d";
  var lastKey = key;

  var powerUpCoords = [];

  var board = Array(rows)
    .fill()
    .map(() => Array(cols).fill(0));

  const startLength = 3;
  function start() {
    document.addEventListener("keydown", (e) => {
      e.preventDefault();
      key = e.key;
    });

    setInterval(() => {
      if (["w", "a", "s", "d"].includes(key)) {
        updatePosition(key);
      } else {
        updatePosition(lastKey);
      }
      // console.log(`X: ${coords.x} Y: ${coords.y}`);
    }, speed);

    setInterval(() => {
      if (powerUpCoords.length <= 2) spawnPowerUp();
    }, 2 * 1000);
  }

  function updatePosition(key) {
    let { x, y } = coords;

    for (let powerUpCoord of powerUpCoords) {
      if (coords.x === powerUpCoord.x && coords.y === powerUpCoord.y) {
        let index = powerUpCoords.indexOf(powerUpCoord);
        console.log(index);
        powerUpCoords.splice(index, 1);
      }
      // console.log(powerUpCoords)
    }

    if (key === "w") {
      board[y][x] = 0;
      y = y == 0 ? rows - 1 : y - 1;
    }
    if (key === "a") {
      board[y][x] = 0;
      x = x == 0 ? cols - 1 : x - 1;
    }
    if (key === "s") {
      board[y][x] = 0;
      y = y == rows - 1 ? 0 : y + 1;
    }
    if (key === "d") {
      board[y][x] = 0;
      x = x == cols - 1 ? 0 : x + 1;
    }
    lastKey = key;
    coords.x = x;
    coords.y = y;
    board[coords.y][coords.x] = 1;
  }

  function spawnPowerUp() {
    let x = Math.floor(Math.random() * cols);
    let y = Math.floor(Math.random() * rows);
    powerUpCoords.push({ x, y });
    board[y][x] = 2;
  }

  start();
</script>

<!-- HTML -->
<div class="game">
  <div class="status">
    <Score />
    <Timer delay={3} />
  </div>
  <div class="board">
    {#each board as row}
      {#each row as col}
        <Cell number={col} />
      {/each}
    {/each}
  </div>
</div>

<!-- Styles -->
<style>
  .game {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .board {
    display: grid;
    grid-template-columns: repeat(15, 40px);
    gap: 3px;
  }
  .status {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
</style>
