<!-- Script -->
<script>
    export let score;
    export let length;
</script>

<!-- HTML -->
<div class="score">
    <h3>Score: {score}</h3>
    <h3>Length: {length}</h3>
</div>

<!-- Styles -->
<style>
    .score {
        display: flex;
        justify-content: space-between;
        width: 200px;
        padding: .1rem 1rem;
        border-radius: .5rem;
        background-color: rgb(37, 35, 47);
    }
    .score:hover {
        cursor: pointer;
    }

    @keyframes grow {
        0% {
            font-size: 10px;
        }
        50% {
            font-size: 30px;
        }
        100% {
            font-size: 25px;
        }
    }
</style>