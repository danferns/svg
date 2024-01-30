<script>
  	// demo: https://svelte.dev/repl/f8b969c1ed5a498d9586d32fe74f8481?version=4.2.9

	function setSizes() {
		STRING_LENGTH = Math.min(window.innerWidth, window.innerHeight) * 0.35; // 35% of screen
		anchorX = window.innerWidth / 2;
		anchorY = window.innerHeight / 2 - STRING_LENGTH / 2;
	}
	
	const TIME_PERIOD = 3; // seconds
	let AMPLITUDE = 0; // initial aplitude in radians
	const DECAY_MULTIPLIER = 0.1; // smaller value -> slower decay

	// setting up the position and size of the pendulum
	let anchorX;
	let anchorY;
	let STRING_LENGTH;
	window.addEventListener("resize", setSizes);
	setSizes();
	
	let initTime = Date.now() / 1000;
	let time = 0;
	
	function updateTime() {
		time = Date.now() / 1000;
		requestAnimationFrame(updateTime);
	}
	
	updateTime();
	
	let theta, bobX, bobY;
	$: decay = Math.E ** (-DECAY_MULTIPLIER * (time - initTime));
	$: theta = Math.cos(2 * Math.PI * (time - initTime) / TIME_PERIOD) * AMPLITUDE * decay;
	$: bobX = anchorX + STRING_LENGTH * Math.sin(theta);
	$: bobY = anchorY + STRING_LENGTH * Math.cos(theta);

</script>

<svelte:window 
	
	on:pointerdown={(e) => {
		AMPLITUDE = Math.atan((anchorX - e.clientX) / (anchorY - e.clientY));
		if (anchorY - e.clientY > 0) {
			AMPLITUDE += Math.PI * Math.sign(e.clientX - anchorX);
		}
		initTime = Date.now() / 1000;
	}}
	
/>

<svg>
	
	<rect height=100% width=100% fill="hsl(180 50% 90%)"></rect>
	
	<circle
		r=10
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
		r=20 
		cx={bobX} 
		cy={bobY}
		fill=orange
		>
	</circle>
	
</svg>

<style>

	:global(body) {
		margin: 0
	}
	
	svg {
		height: 100%;
		width: 100%;
	}
	
</style>
