<script>
	export let product = {
		id: null,
		title: '',
		description: '',
		price: 0,
		discountPercentage: 0,
		rating: 0,
		stock: 0,
		brand: '',
		category: '',
		thumbnail: ''
	};
</script>

<div class="card mb-4">
	<img src={product.thumbnail} class="card-img-top" alt={product.title} />
	<div class="card-body">
		<h4 class="card-title">{product.title}</h4>
		<p class="mb-4">
			<b>${product.price}</b>

			{#if product.discountPercentage}
				<span class="badge rounded-pill text-bg-danger">-{product.discountPercentage}%</span>
			{/if}
		</p>
		<span class="badge rounded-pill text-bg-warning"
			><i class="fa-solid fa-star" /> {product.rating}</span
		>
	</div>
	<ul class="list-group list-group-flush">
		<li class="list-group-item"><b>Brand:</b> {product.brand}</li>
		<li class="list-group-item"><b>Category:</b> {product.category}</li>
		<li class="list-group-item"><b>Stock:</b> {product.stock} units</li>
	</ul>
</div>

<style>
	.card {
		width: 400px;
		padding: 12px;
	}

	.card-img-top {
		width: 100%;
		height: 200px;
		object-fit: cover;
	}
</style>
