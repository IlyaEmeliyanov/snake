<!-- Scripts -->
<script>
    export let gameStatus;
    export let restart;
</script>

<!-- HTML -->
<div class="game-status">
    <h1 class="game-status-title">{gameStatus}</h1>
    <nav>
        <ul>
            <button on:click={restart()} class="game-status-btn">
                <img src="/images/refresh.svg" alt="refresh-icon" />
            </button>
        </ul>
    </nav>
</div>

<!-- Styles -->
<style>
    @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@700&family=Zen+Kaku+Gothic+Antique:wght@300;400;500;700;900&display=swap');

  .game-status {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 100;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: rgb(0, 0, 0, 0.7);
  }
  .game-status-title {
    width: 100%;
    text-align: center;
    font-size: 250px;
    font-family: 'Montserrat', sans-serif;
    font-weight: 700;
    text-transform: uppercase;
    color: rgba(255, 255, 255, 0.5);
    animation: 2s ease-in-out grow infinite;
  }
  @keyframes grow {
      0% {
          transform: scale(1);
      }
      30% {
        transform: scale(1.2);
      }
      60% {
        transform: scale(1);
      }
      80% {
        transform: scale(1.2);
      }
      100% {
        transform: scale(1);
      }
  }
  .game-status-btn {
    margin: 0;
    background: transparent;
    border: none;
    border-radius: 5px;
    padding: 1rem;
    border-radius: 0.5rem;
    background-color: rgb(37, 35, 47);
  }

</style>
