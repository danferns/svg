<script>
	let time = Date.now();
	
	function updateTime() {
		time = Date.now() / 1000;
		requestAnimationFrame(updateTime);
	}
  
	updateTime();
</script>

<svg>

</svg>

<style>
	svg {
		height: 100%;
		width: 100%;
	}
</style>
