<script>
    export let x;
    export let y;

    function handleMousedown() {
        window.addEventListener("pointermove", handleMousemove);
        window.addEventListener("pointerup", handleMouseup);
    }
    function handleMousemove(event) {
        x += event.movementX;
        y += event.movementY;
    }

    function handleMouseup(event) {
        window.removeEventListener("pointermove", handleMousemove);
        window.removeEventListener("pointerup", handleMouseup);
    }
</script>

<circle r="10" cx={x} cy={y} on:pointerdown={handleMousedown} />

<style>
    circle:active {
        fill: hsl(0 0% 40%);
    }
</style>
