<script>
    // demo: https://svelte.dev/repl/0eae439c77014067b26e9eb3bab384c6?version=3.47.0
    let indices = Array(100)
        .fill(0)
        .map((val, i) => i);

    let mouseX = 0;
    let mouseY = 0;

    let height = document.body.clientHeight;
    let width = document.body.clientWidth;
</script>

<svelte:window
    on:mousemove={(e) => {
        mouseX = e.offsetX;
        mouseY = e.offsetY;
    }}
    on:resize={() => {
        height = document.body.clientHeight;
        width = document.body.clientWidth;
    }}
/>

<svg>
    {#each indices as i}
        {@const x = i % 10}
        {@const y = Math.floor(i / 10)}
        {@const distance =
            ((x + 1 / 4 - (mouseX / width) * 10) ** 2 +
                (y + 1 / 4 - (mouseY / height) * 10) ** 2) **
            (1 / 2)}
        <circle
            r={10 - 0.75 * distance}
            cx="calc({x + 0.5} * 10%)"
            cy="calc({y + 0.5} * 10%)"
            fill="hsla(0 0% {(1 - distance * 0.1) * 50}%)"
        />
    {/each}
</svg>

<style>
    svg {
        height: 100%;
        width: 100%;
    }
</style>
