<script>
	type Cicrle = {
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
</script>

<h1>Circle Drawer</h1>
<div></div>

<style>
</style>
