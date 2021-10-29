<!-- Script -->
<script>
    export let delay; // delay set in seconds

    let timeCount = delay;
    // let time = 30;
    function timer() {
        setInterval(() => {
            if (timeCount >= 1)
                timeCount -= 1;
            else
                return;
        }, 1000);
    }

    timer();
</script>

<!-- HTML -->
<h3>{timeCount} sec</h3>

<!-- Styles -->