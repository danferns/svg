<script>
  // demo: https://svelte.dev/repl/80ddad7d66ce4331bfd119707a3d6d66?version=3.47.0
	// drag the anchor (black circle) around to move the pendulum
	// click anywhere on the background to set the initial amplitude
	
	const TIME_PERIOD = 5; // seconds
	let AMPLITUDE = Math.PI / 3; // initial aplitude in radians
	const STRING_LENGTH = "35vmin"; // css length
	const DECAY_MULTIPLIER = 0.01; // smaller value -> slower decay
	
	let anchorX = window.innerWidth / 2;
	let anchorY = window.innerHeight / 2;
	
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
	$: bobX = `calc(${anchorX}px + ${STRING_LENGTH} * ${Math.sin(theta)})`;
	$: bobY = `calc(${anchorY}px + ${STRING_LENGTH} * ${Math.cos(theta)})`;
	
	// panning
	
	function handleMousedown(event) {
		anchorX = event.clientX;
		anchorY = event.clientY;

		window.addEventListener('mousemove', handleMousemove);
		window.addEventListener('mouseup', handleMouseup);
	}

	function handleMousemove(event) {
		anchorX = event.clientX;
		anchorY = event.clientY;
	}
	
	function handleMouseup(event) {
		anchorX = event.clientX;
		anchorY = event.clientY;

		window.removeEventListener('mousemove', handleMousemove);
		window.removeEventListener('mouseup', handleMouseup);
	}
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
		on:pointerdown|stopPropagation={handleMousedown}
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
	svg {
		height: 100%;
		width: 100%;
	}
</style>
