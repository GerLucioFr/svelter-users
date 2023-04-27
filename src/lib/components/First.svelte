<script>
	let name = 'Ger';
	let numberOne = 0;
	let numberTwo = 0;
	let disabled = false;
	let clicksCounter = 0;
	let counter = 0;
	let names = [];
	$: result = counter * 10;

	let users = [];
	let promise = getData();
	async function getData() {
		const res = await fetch('https://jsonplaceholder.typicode.com/users');
		users = await res.json();

		if (res.ok) {
			return users;
		} else {
			throw new Error('Error getting data');
		}
	}

	const alertFunction = () => {
		clicksCounter++;
		alert(`You clicked this button for ${clicksCounter} times`);
	};
</script>

<main>
	<h1>Welcome to SvelteKit {name}</h1>
	<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
	<input type="text" name="name" id="name" bind:value={name} />
	<hr />
	<input type="range" name="numberOne" id="numberOne" bind:value={numberOne} />
	<input type="range" name="numberTwo" id="numberTwo" bind:value={numberTwo} />

	<h4>
		{numberOne} + {numberTwo} = {numberOne + numberTwo}
	</h4>

	<hr />

	<input type="checkbox" name="disabled" id="disabled" bind:checked={disabled} />
	<label for="disabled">Enable button?</label>

	<button type="button" disabled={!disabled} on:click={alertFunction}>I'm a button</button>
	<button
		type="button"
		disabled={!disabled}
		on:click={() => {
			alert(`Other function :)`);
		}}>Other button</button
	>

	<hr />

	<h1>Reactive vars</h1>
	<button type="button" on:click={() => counter++}>Increment</button>
	<h3>Counter: {counter}</h3>
	<h3>Counter x10: {result}</h3>

	<hr />

	<h3>IF</h3>
	{#if disabled}
		<h4>Is disabled</h4>
	{:else}
		<h4>Is enabled</h4>
	{/if}
	<hr />

	<h3>Each (bucle)</h3>

	<ul>
		{#each names as name, i}
			<li>{i + 1} - {name.nombre} {name.apellido}</li>
		{:else}
			<h4>No data found</h4>
		{/each}
	</ul>

	<hr />

	<h3>Await</h3>
	{#await promise}
		<p>Getting data...</p>
	{:then users}
		<ul>
			{#each users as user}
				<li>{user.id} - {user.username}: {user.name}</li>
			{:else}
				<h4>No data found</h4>
			{/each}
		</ul>
	{:catch error}
		<p>{error}</p>
	{/await}
</main>
