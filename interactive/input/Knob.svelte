<script>
    let size = 100; // pixels
    let percent = 50;

    let x,
        y,
        meterRadius = size * 0.8,
        startAngle = 210,
        endAngle;
    $: endAngle = startAngle + ((360 - startAngle) * 2 * percent) / 100;
    let start, end, drawLargerArc;
    x = window.innerWidth / 2;
    y = window.innerHeight / 2;

    $: start = polarToCartesian(x, y, meterRadius, endAngle);
    $: end = polarToCartesian(x, y, meterRadius, startAngle);
    $: drawLargerArc = endAngle - startAngle > 180 ? "1" : "0";

    function boundValue(val, min, max) {
        return Math.min(Math.max(val, min), max);
    }

    function polarToCartesian(centerX, centerY, radius, degrees) {
        const radians = ((degrees - 90) * Math.PI) / 180.0;

        return {
            x: centerX + radius * Math.cos(radians),
            y: centerY + radius * Math.sin(radians),
        };
    }
</script>

<svg
    on:pointermove|stopPropagation={(e) => {
        if (e.buttons === 1) {
            percent = boundValue(percent - (50 * e.movementY) / size, 0, 100);
        }
    }}
>
    <circle r={size} cx={x} cy={y} />
    <path
        d="M {start.x} {start.y} A {meterRadius} {meterRadius} 0 {drawLargerArc} 0 {end.x} {end.y}"
        stroke-width={size / 12}
    />
</svg>

<style>
    svg {
        height: 100%;
        width: 100%;
        touch-action: none;
    }

    circle {
        fill: hsl(0 0% 10%);
    }

    path {
        fill: none;
        stroke: hsl(180 50% 50%);
        stroke-linecap: round;
    }
</style>
