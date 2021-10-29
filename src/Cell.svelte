<!-- Script -->
<!-- Script -->
<script>
    export let number;

    console.log(number);
    const cell = document.querySelector(".cell");

    function styleCell(number) {
        switch(number) {
            case 1:
                return "snake-cell cell";
            case 2:
                return "power-up-cell cell";
            default:
                return "cell";
        }
    }
</script>

<!-- HTML -->
<div class={styleCell(number)}>
</div>

<!-- Styles -->
<style>
    .cell {
        width: 2.5rem;
        height: 2.5rem;
        border-radius: 4px;
        background: rgb(74, 70, 87);
        box-shadow: rgb(255, 255, 255, .1);
        /* transition: .3s cubic-bezier(1, 0, 0, 1); */
        /* transition: .1s; */
        animation: grow .2s;
    }
    @keyframes grow {
        0% {
            transform: scale(0);
        }
        100% {
            transform: scale(1);
        }
    }

    .snake-cell {
        background-color: rgb(255, 0, 123);
        box-shadow: 0 0 2rem 0 rgb(255, 0, 123, .2);
    }

    .power-up-cell {
        background-color: rgb(0, 255, 145);
        box-shadow: 0 0 2rem 0 rgba(86, 255, 182, 0.2);
    }
</style>