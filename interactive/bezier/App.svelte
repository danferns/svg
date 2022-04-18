<script>
    import ControlPoint from "./ControlPoint.svelte";

    let x1 = 10,
        y1 = 200,
        x2 = 300,
        y2 = 100,
        x3 = 50,
        y3 = 60;
</script>

<svg>
    <path d="M{x1} {y1} C {x1} {y1} {x3} {y3} {x2} {y2}" />

    <ControlPoint bind:x={x1} bind:y={y1} />
    <ControlPoint bind:x={x2} bind:y={y2} />
    <ControlPoint bind:x={x3} bind:y={y3} />
</svg>

<style>
    svg {
        height: 100%;
        width: 100%;
        touch-action: none;
    }

    path {
        stroke: black;
        stroke-width: 4px;
        fill: none;
    }
</style>
