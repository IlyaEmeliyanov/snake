<!-- Script -->
<!-- Script -->
<script>
    export let number;

    function styleCell(number) {
        switch(number) {
            case 1:
                return "snake-cell cell";
            case 2:
                return "power-up-cell cell";
            case 3:
                return "super-power-up-cell cell";
            case 4:
                return "danger-cell cell";
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
        box-shadow: rgb(255, 255, 255, .2);
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
        box-shadow: 0 0 2rem 0 rgb(255, 0, 123, .5);
    }

    .power-up-cell {
        background-color: rgb(0, 255, 145);
        box-shadow: 0 0 2rem 0 rgba(86, 255, 182, 0.5);
    }
    .super-power-up-cell {
        background-color: rgb(255, 234, 0);
        box-shadow: 0 0 2rem 0 rgba(232, 255, 86, 0.8);
    }

    .danger-cell {
        background-color: rgba(125, 20, 254);
        box-shadow: 0 0 2rem 0 rgba(255, 147, 248, 0.5);
    }
</style>