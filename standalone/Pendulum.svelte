<script>
  // demo: https://svelte.dev/repl/cc4e5a8fe4a1416f8613902565f46634?version=3.47.0
	const TIME_PERIOD = 5; // seconds
	const AMPLITUDE = Math.PI / 3; // radians
	const STRING_LENGTH = "35vmin"; // css length
	
	let anchorX = window.innerWidth / 2;
	let anchorY = window.innerHeight / 2;
	
	let time = 0;
	
	function updateTime() {
		time = Date.now() / 1000;
		requestAnimationFrame(updateTime);
	}
	
	updateTime();
	
	let theta, bobX, bobY;
	$: theta = Math.cos(2 * Math.PI * time / TIME_PERIOD) * AMPLITUDE;
	$: bobX = `calc(${anchorX}px + ${STRING_LENGTH} * ${Math.sin(theta)})`;
	$: bobY = `calc(${anchorY}px + ${STRING_LENGTH} * ${Math.cos(theta)})`;
</script>

<svelte:window 
	on:resize={() => {
		anchorX = window.innerWidth / 2;
		anchorY = window.innerHeight / 2;
	}}
/>

<svg>
	
	<rect height=100% width=100% fill="hsl(180 50% 90%)"></rect>
	
	<circle
		r=5
		cx={anchorX}
		cy={anchorY}
		>
	</circle>
	
	<line
		stroke="black"
		stroke-width=4px
		x1={anchorX}
		y1={anchorY}
		x2={bobX}
		y2={bobY}
		>
	</line>
	
	<circle 
		r=15 
		cx={bobX} 
		cy={bobY}
		fill=orange
		>
	</circle>
	
</svg>

<style>
	svg {
		height: 100%;
		width: 100%;
	}
</style>
