<script lang="ts">
	type Circle = {
		id: string;
		cx: number;
		cy: number;
		r: number;
	};
	type Status = 'drawing' | 'editing';

	let status = $state<Status>('drawing');
	let circles = $state<Circle[]>([]);
	let selected = $state<Circle>()!;

	let snapshots: Circle[][] = [];
	let history = $state(-1);

	function drawCircle(e: MouseEvent) {
		if (status === 'editing') {
			snapshot();
			status = 'drawing';
			return;
		}

		const svgEl = e.target as SVGElement;
		const { left, top } = svgEl.getBoundingClientRect();

		const circle = {
			id: window.crypto.randomUUID(),
			cx: +(e.clientX - left).toFixed(),
			cy: +(e.clientY - top).toFixed(),
			r: 40
		};

		circles.push(circle);
		selected = circle;

		snapshot();
	}

	function undo() {
		circles = snapshots[--history];
	}

	function redo() {
		circles = snapshots[++history];
	}

	function snapshot() {
		history++;
		snapshots.push($state.snapshot(circles));
	}
</script>

<h1>Circle Drawer</h1>
<div class="space-y">
	<div class="actions flex-center">
		<button onclick={undo} disabled={history === -1}>Undo</button>
		<button onclick={redo} disabled={history === snapshots.length - 1}>Redo</button>
	</div>

	{#if status === 'editing'}
		<div class="adjust space-y">
			<span>Adjust diameter of circle at ({selected.cx}, {selected.cy})</span>
			<input type="range" bind:value={selected.r} />
		</div>
	{/if}

	<!-- svelte-ignore a11y_click_events_have_key_events -->
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<svg onclick={drawCircle} viewBox="0 0 600 400">
		{#each circles as circle}
			<circle
				{...circle}
				fill={selected.id === circle.id ? '#444' : 'transparent'}
				stroke="#fff"
				stroke-width="2"
				onclick={(e) => {
					e.stopPropagation();
					selected = circle;
				}}
				oncontextmenu={(e) => {
					if (status === 'editing') {
						snapshot();
					}
					e.preventDefault();
					status = 'editing';
					selected = circle;
				}}
			></circle>
		{/each}
	</svg>
</div>

<style>
	svg {
		width: 600px;
		height: 400px;
		border: 2px solid gray;
	}

	.adjust {
		width: 400px;
		position: absolute;
		top: 50%;
		left: 50%;
		translate: -50% -50%;
		padding: 2rem;
		text-align: center;
		border-radius: 20px;
		backdrop-filter: blur(5px);
	}
</style>
