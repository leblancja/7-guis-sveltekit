<script lang="ts">
	let duration: number = $state(0);
	let elapsed: number = $state(0);
	let interval: number;

	function startTimer() {
		interval = setInterval(() => {
			elapsed += 0.1;
			if (elapsed > duration) {
				elapsed = duration;
				clearInterval(interval);
			}
		}, 100);
	}

	function reset() {
		elapsed = 0;
		clearInterval(interval);
		startTimer();
	}

	$effect(() => {
		if (!duration) return;
		startTimer();
		return () => clearInterval(interval);
	});
</script>

<h1>Timer</h1>
<div class="container">
	<div>
		<label>
			<span>Elapsed time:</span>
			<progress max={duration} value={elapsed}></progress>
		</label>
		<div>{elapsed.toFixed(1)}s</div>
	</div>
	<label>
		<span>Duration:</span>
		<input type="range" bind:value={duration} min="1" max="10" />
	</label>
	<button onclick={reset}>Reset</button>
</div>
