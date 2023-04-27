<script>
	import Card from '$lib/components/Design/Card.svelte';
	import photo from '$lib/assets/img/image.jpg';

	let users = [];
	let promise = getData();
	async function getData() {
		const res = await fetch('https://jsonplaceholder.typicode.com/users');
		users = await res.json();

		if (res.ok) {
			return users.map((user) => {
				return {
					...user,
					photo
				};
			});
		} else {
			throw new Error('Error getting data');
		}
	}
</script>

<a href="/add" class="btn btn-primary mt-2 mb-5">Add User</a>
<div class="row justify-content-between">
	{#await promise}
		<p>Getting data...</p>
	{:then users}
		{#each users as user}
			<Card {user} />
		{:else}
			<h4>No data found</h4>
		{/each}
	{:catch error}
		<p>{error}</p>
	{/await}
</div>
