<script lang="ts">
	type Person = {
		first: string;
		last: string;
	};

	let people = $state([
		{ first: 'Jordan', last: 'LeBlanc' },
		{ first: 'Rich', last: 'Harris' },
		{ first: 'Evan', last: 'You' }
	]);

	let selected = $state<Person>();
	let person = $state<Person>({ first: '', last: ' ' });

	let prefix = $state('');
	let filteredPeople = $derived(
		prefix ? people.filter((p) => p.last.toLowerCase().startsWith(prefix)) : people
	);

	$effect(() => {
		person = {
			first: selected?.first ?? '',
			last: selected?.last ?? ''
		};
	});

	function createPerson() {
		people.push(person);
		clearFields();
	}

	function updatePerson() {
		const index = people.indexOf(selected!);
		people[index] = { first: person.first, last: person.last };
	}

	function deletePerson() {
		people = people.filter((p) => p.first !== person.first || p.last !== person.last);
		clearFields();
	}

	function clearFields() {
		person = { first: '', last: '' };
	}
</script>

<h1>CRUD</h1>
<div class="contain">
	<div class="search">
		<label class="group">
			<span>Filter prefix:</span>
			<input type="text" bind:value={prefix} />
		</label>
	</div>

	<div class="people">
		<span>Names:</span>
		<select bind:value={selected} size="5">
			{#each filteredPeople as person}
				<option value={person}>{person.last}, {person.first}</option>
			{/each}
		</select>
	</div>

	<div class="details">
		<label class="group">
			<span>First:</span>
			<input type="text" bind:value={person.first} />
		</label>

		<label class="group">
			<span>Last:</span>
			<input type="text" bind:value={person.last} />
		</label>
	</div>

	<div class="actions">
		<button onclick={createPerson}>Create</button>
		<button onclick={updatePerson}>Update</button>
		<button onclick={deletePerson}>Delete</button>
	</div>
</div>

<style>
	.contain {
		width: 500px;
		display: grid;
		grid-template-areas: 'search .' 'people details' 'actions actions';
		grid-template-columns: 240px 1fr;
		gap: 1rem;
		padding: 1rem;
		grid-auto-rows: auto;

		.search {
			grid-area: search;

			.group {
				display: grid;
				grid-template-columns: 2fr 1fr;
				align-items: baseline;
			}
		}

		.people {
			grid-area: people;
		}

		.details {
			grid-area: details;
			display: grid;
			grid-template-columns: auto 1fr;
			grid-template-rows: repeat(2, auto) 1fr;
			align-items: baseline;
			gap: 1rem;

			.group {
				display: contents;
			}
		}

		.actions {
			grid-area: actions;
		}
	}
</style>
