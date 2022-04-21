<script>
    let radius = 100; // pixels
    let percent = 50;

    let x,
        y,
        meterRadius = radius * 1.1,
        startAngle = 210,
        endAngle;
    $: endAngle = startAngle + ((360 - startAngle) * 2 * percent) / 100;
    x = radius * 1.2;
    y = radius * 1.2;

    let start, end, drawLargerArc;
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
            percent = boundValue(percent - (50 * e.movementY) / radius, 0, 100);
        }
    }}
    height={radius * 2.4}
    width={radius * 2.4}
>
    <circle r={radius} cx={x} cy={y} />
    <path
        d="M {start.x} {start.y} A {meterRadius} {meterRadius} 0 {drawLargerArc} 0 {end.x} {end.y}"
        stroke-width={radius / 12}
    />
    <text {x} y={y + radius * 0.15} font-size={radius * 0.5}>{Math.round(percent)}%</text>
</svg>

<style>
    svg {
        touch-action: none;
        user-select: none;
    }

    circle {
        fill: hsl(0 0% 10%);
        transition: fill 0.2s;
    }

    circle:active {
        fill: hsl(0 0% 20%);
    }

    path {
        fill: none;
        stroke: hsl(180 50% 50%);
        stroke-linecap: round;
        transition: stroke 0.2s;
    }

    circle:active + path {
        stroke: hsl(180 70% 70%);
    }

    text {
        font-family: Arial, sans-serif;
        fill: hsl(0 0% 80%);
        text-anchor: middle;
        pointer-events: none;
    }
</style>
