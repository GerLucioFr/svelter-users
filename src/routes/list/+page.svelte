<script>
	import Card from '$lib/components/Design/Card.svelte';
	import photo from '$lib/assets/img/image.jpg';

	let products = [];
	let promise = getData();
	async function getData() {
		const res = await fetch('https://dummyjson.com/products');
		const data = await res.json();
		products = data.products;

		if (res.ok) {
			return products.map((product) => {
				return {
					...product,
					photo
				};
			});
		} else {
			throw new Error('Error getting data');
		}
	}
</script>

<a href="/add" class="btn btn-primary mt-2 mb-5">Add product</a>
<div class="row justify-content-between">
	{#await promise}
		<p>Getting data...</p>
	{:then products}
		{#each products as product}
			<Card {product} />
		{:else}
			<h4>No data found</h4>
		{/each}
	{:catch error}
		<p>{error}</p>
	{/await}
</div>
