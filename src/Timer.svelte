<!-- Script -->
<script>
    export let delay; // delay set in seconds

    let timeCount = delay;
    // let time = 30;
    function timer() {
        setInterval(() => {
            if (timeCount >= 1)
                timeCount -= 1;
            else {
                for (let i = 0; i < 100; i++)
                    window.clearInterval();
                return;
            }
        }, 1000);
    }

    timer();
</script>

<!-- HTML -->
<div class="timer">
    <img class="icon" src="/images/stopwatch.svg" alt="stopwatch-icon">
    <h3>{timeCount} sec</h3>
</div>

<!-- Styles -->
<style>

    .timer {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: .1rem 1rem;
        border-radius: .5rem;
        background-color: rgb(37, 35, 47);
    }
</style>