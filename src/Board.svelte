<!-- Scripts -->
<script>
  import Score from "./Score.svelte";
  import GameStatus from "./GameStatus.svelte";
  import Cell from "./Cell.svelte";

  // Game vars
  $: gameStatus = "Playing";

  // Table vars
  var rows = 15;
  var cols = 15;

  // Coords
  var coords = [
    { x: 2, y: 0 },
    { x: 1, y: 0 },
    { x: 0, y: 0 },
  ];
  $: powerUpCoords = [];

  // Keys
  var key = "d";
  var lastKey = key;

  // Snake config
  $: speed = 0.5;
  var delay = 100; // delay in ms for refresh
  var score = 0;

  // Timer config
  var time = 30; // in seconds
  $: curTime = time;

  var board = Array(rows)
    .fill()
    .map(() => Array(cols).fill(0));

  document.addEventListener("keydown", (e) => {
    e.preventDefault();
    key = e.key;
  });

  function start() {
    // Change the position of the snake
    setInterval(() => {
      if (gameStatus == "Playing") {
        if (["w", "a", "s", "d"].includes(key)) {
          updatePosition(key);
        } else {
          updatePosition(lastKey);
        }
      } else if (gameStatus === "Game over" || time <= 0) {
        gameOver();
      }
    }, delay);

    // Randomly spawns a powerup or powerdown on the grid
    setInterval(() => {
      if (powerUpCoords.length <= 3) spawnPowerUp();
    }, speed * 1000);

    // Timer
    setInterval(() => {
      if (curTime >= 1) curTime -= 1;
      else {
        gameOver();
      }
    }, 1000);
  }

  function updatePosition(key) {
    let head = coords[0];
    if (head) {
      // if head exists then move it
      let { x, y } = head; // head coordinates that will change
      let { x: whiteSpaceX, y: whiteSpaceY } = coords[0]; // head coordinates that will not change

      powerupCollision(x, y); // describe power-up collision

      if (key === "w" && lastKey !== "s") {
        y = y == 0 ? rows - 1 : y - 1;
      }
      if (key === "a" && lastKey !== "d") {
        x = x == 0 ? cols - 1 : x - 1;
      }
      if (key === "s" && lastKey !== "w") {
        y = y == rows - 1 ? 0 : y + 1;
      }
      if (key === "d" && lastKey !== "a") {
        x = x == cols - 1 ? 0 : x + 1;
      }
      lastKey = key;

      // Change the other coordinates
      let snakeLength = coords.length;
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
    } else {
      gameStatus = "Game over";
    }
  }

  function spawnPowerUp() {
    let choice = Math.floor(Math.random() * 5) + 2; // spawning a random block at the beginning
    let x = Math.floor(Math.random() * cols);
    let y = Math.floor(Math.random() * rows);

    for (let coord of coords) {
      if (coord.x === x && coord.y === y) {
        console.log(`Equal x and y: ${x} ${y}`);
        while (coord.x !== x && coord.y !== y) {
          x = Math.floor(Math.random() * cols);
          y = Math.floor(Math.random() * rows);
        }
      }
    }

    const badPowerupCoords = powerUpCoords.filter((p) => p.id === 4);
    if (badPowerupCoords.length >= 2)
      choice = Math.floor(Math.random() * 2) + 2; // to avoid the choosing the bad block

    switch (choice) {
      case 2: // for green
        powerUpCoords.push({
          x,
          y,
          scoreIncrease: 10,
          lengthIncrease: 1,
          id: 2,
        });
        board[y][x] = 2;
        break;
      case 3: // for yellow
        powerUpCoords.push({
          x,
          y,
          scoreIncrease: 50,
          lengthIncrease: 2,
          id: 3,
        });
        board[y][x] = 3;
        break;
      default:
        // for pink one
        powerUpCoords.push({
          x,
          y,
          scoreIncrease: -20,
          lengthIncrease: -2,
          id: 4,
        });
        board[y][x] = 4;
        break;
    }
  }
  function powerupCollision(x, y) {
    // Trigger power-up collision
    for (const powerUpCoord of powerUpCoords) {
      if (x === powerUpCoord.x && y === powerUpCoord.y) {
        let index = powerUpCoords.indexOf(powerUpCoord);
        for (let i = 0; i < powerUpCoord.lengthIncrease; i++)
          coords.push({ x, y });

        const scoreIncrease = powerUpCoord.scoreIncrease;
        const newScore = score + scoreIncrease;
        if (newScore >= 0) score = newScore;
        powerUpCoords.splice(index, 1);

        if (powerUpCoord?.id === 4) {
          const deadCoord = coords.pop();
          const deadCoord2 = coords.pop();
          if (coords.length > 1) {
            board[deadCoord.y][deadCoord.x] = 0;
            board[deadCoord2.y][deadCoord2.x] = 0;
          } else {
            gameOver();
            return;
          }
        } else if (powerUpCoord?.id === 5) {
          speed *= 2;
        }
      }
    }
  }

  function restart() {
    gameStatus = "Playing";
    coords = [
      { x: 2, y: 0 },
      { x: 1, y: 0 },
      { x: 0, y: 0 },
    ];
    board = Array(rows)
      .fill()
      .map(() => Array(cols).fill(0));
    powerUpCoords = [];
    curTime = time;
    clearAllIntervals();
    start();
  }

  function playPause() {}

  function gameOver() {
    gameStatus = "Game over";
    clearAllIntervals()
  }

  function clearAllIntervals() {
    for (let i = 0; i < 100; i++) {
      window.clearInterval(i);
    }
  }

  start();
</script>

<!-- HTML -->
<div>
  {#if gameStatus == "Game over" || gameStatus == "You win"}
    <GameStatus {restart} {gameStatus} />
  {/if}
  <div class="game">
    <div class="status">
      <Score {score} length={coords.length - 1} />
      <div style="display: flex; justify-content: center; gap: 10px;">
        <button on:click={restart} class="btn-status"
          ><img src="/images/refresh.svg" alt="refresh-icon" /></button
        >
        <button on:click={playPause} class="btn-status"
          ><img src="/images/pause.svg" alt="pause-icon" /></button
        >
      </div>
      <div class="timer">
        <img class="icon" src="/images/stopwatch.svg" alt="stopwatch-icon" />
        <h3>{curTime} sec</h3>
      </div>
    </div>
    <div class="board">
      {#each board as row}
        {#each row as col}
          <Cell number={col} />
        {/each}
      {/each}
    </div>
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

  .timer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.1rem 1rem;
    border-radius: 0.5rem;
    background-color: rgb(37, 35, 47);
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
</style>
