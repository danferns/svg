<script>
  // demo: https://svelte.dev/repl/3a947446f20f47fdb02e7cfc3ea38d3a?version=3.47.0
	let indices = Array(4)
        .fill(0)
        .map((val, i) => i + 1);
	
	let time = 0;
	function updateTime() {
		time += 0.01;
		requestAnimationFrame(updateTime)
	}
	requestAnimationFrame(updateTime);
</script>

<svg>
	<rect fill="hsl(230 50% 50%)" width=100% height=100%></rect>
	{#each indices as i}
		<circle r={i ** (Math.sin(time * 2 + Math.PI * Math.sin(time))) * 140 * Math.abs(Math.sin(time))} cx=50% cy=50% fill="hsla(220, 0%, 80%, 10%)"></circle>
	{/each}
</svg>

<style>
	svg {
		height: 100%;
		width: 100%;
	}
</style>
