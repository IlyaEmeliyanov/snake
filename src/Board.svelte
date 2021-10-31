<!-- Scripts -->
<script>
  import Score from "./Score.svelte";
  import Timer from "./Timer.svelte";
  import Cell from "./Cell.svelte";

  // Table vars
  var rows = 15;
  var cols = 15;

  // Coords
  var coords = [
    { x: 2, y: 0 },
    { x: 1, y: 0 },
    { x: 0, y: 0 },
  ];
  var powerUpCoords = [];

  // Keys
  var key = "d";
  var lastKey = key;

  // Snake config
  var delay = 100; // delay in ms for refresh
  var score = 0;

  // Timer config
  var time = 30;

  var board = Array(rows)
    .fill()
    .map(() => Array(cols).fill(0));

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
    }, delay);

    setInterval(() => {
      if (powerUpCoords.length <= 1) spawnPowerUp();
    }, 2 * 1000);
  }

  function updatePosition(key) {
    let { x, y } = coords[0]; // head coordinates that will change
    let { x: whiteSpaceX, y: whiteSpaceY } = coords[0]; // head coordinates that will not change

    let snakeLength = coords.length;

    for (const powerUpCoord of powerUpCoords) {
      if (x === powerUpCoord.x && y === powerUpCoord.y) {
        let index = powerUpCoords.indexOf(powerUpCoord);
        for (let i = 0; i < powerUpCoord.lengthIncrease; i++) 
          coords.push({ x, y });
        score += powerUpCoord.scoreIncrease;
        powerUpCoords.splice(index, 1);
      }
    }

    if (key === "w" && lastKey !== "s") {
      y = y == 0 ? rows - 1 : y - 1;
    }
    if (key === "a" && lastKey !== "d") {
      console.log(`Key: ${key}`);
      console.log(`Last key: ${lastKey}`);
      x = x == 0 ? cols - 1 : x - 1;
    }
    if (key === "s" && lastKey !== "w") {
      y = y == rows - 1 ? 0 : y + 1;
    }
    if (key === "d" && lastKey !== "a") {
      x = x == cols - 1 ? 0 : x + 1;
    }
    lastKey = key;

    for (let i = 0; i < snakeLength - 1; i++) {
      // Reassign to new coordinates
      coords[i] = { x, y };

      // Rerender the board
      board[coords[i].y][coords[i].x] = 1;
      board[whiteSpaceY][whiteSpaceX] = 0;

      // Change the new position of the next block
      x = whiteSpaceX;
      y = whiteSpaceY;

      // Change also the last position of the white space
      whiteSpaceX = coords[i + 1].x;
      whiteSpaceY = coords[i + 1].y;
    }
  }

  function spawnPowerUp() {
    const choice = Math.floor(Math.random() * 4) + 2;
    let x = Math.floor(Math.random() * cols);
    let y = Math.floor(Math.random() * rows);
    switch (choice) {
      case 2:
        powerUpCoords.push({ x, y, scoreIncrease: 10, lengthIncrease: 1 });
        board[y][x] = 2;
        break;
      case 3:
        powerUpCoords.push({ x, y, scoreIncrease: 50, lengthIncrease: 2 });
        board[y][x] = 3;
        break;
      case 4:
        break;
      default:
        powerUpCoords.push({ x, y });
        board[y][x] = 2;

        break;
    }
  }

  function restart() {}

  function playPause() {
    console.log("game paused");
  }

  start();
</script>

<!-- HTML -->
<div class="game">
  <div class="status">
    <Score {score} length={coords.length - 1} />
    <div style="display: flex; justify-content: center; gap: 10px;">
      <button on:click={restart()} class="btn-status"
        ><img src="/images/refresh.svg" alt="refresh-icon" /></button
      >
      <button on:click={playPause()} class="btn-status"
        ><img src="/images/pause.svg" alt="pause-icon" /></button
      >
    </div>
    <Timer delay={time} />
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
    margin-bottom: 1rem;
  }

  .btn-status {
    margin: 0;
    background: transparent;
    border: none;
    border-radius: 5px;
    padding: 1rem;
    border-radius: 0.5rem;
    background-color: rgb(37, 35, 47);
  }
  img svg path {
    color: #a8a1b8;
  }
</style>
