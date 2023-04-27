<script>
	import * as yup from 'yup';
    import { goto } from '$app/navigation';

	const maxSize = 1024 * 1024 * 5; //5MB
	const validFormats = ['image/jpeg', 'image/png'];

	let formData = {
		title: '',
		description: '',
		price: 0,
		discountPercentage: 0,
		stock: 0,
		brand: '',
		category: '',
		thumbnail: null
	};
	let errors = {};

	const schema = yup.object().shape({
		title: yup.string().required().min(3).max(128),
		description: yup.string().required().min(3).max(512),
		price: yup.number().positive(),
		discountPercentage: yup.number().positive().min(0.01).max(100),
		stock: yup.number().positive().integer(),
		brand: yup.string().required().min(3).max(128),
		category: yup.string().required().min(3).max(128),
		thumbnail: yup
			.mixed()
			.required()
			.test('is-big-file', 'Image size should not be grater than 5Mb', isValidSize)
			.test('is-incorrect-format', 'Image type should be jpg or png', isValidFormat)
	});

	async function handleSubmit(event) {
		event.preventDefault();
		const file = new FormData(event.target).get('thumbnail');

		try {
			const validation = await schema.validate(
				{ ...formData, thumbnail: file },
				{ abortEarly: false }
			);

			if (validation) {
				errors = {};
				addProduct(validation);
			}
		} catch (error) {
			const validationErrors = new yup.ValidationError(error);
			errors = validationErrors.inner.reduce(
				(acc, curr) => ({ ...acc, [curr.path]: curr.message }),
				{}
			);
			console.log(errors);
		}
	}

	function isValidSize(file) {
		return file.size <= maxSize;
	}

	function isValidFormat(file) {
		return validFormats.includes(file.type);
	}
	async function addProduct(data) {
		try {
			const res = await fetch('https://dummyjson.com/products/add', {
				method: 'POST',
				headers: { 'Content-Type': 'application/json' },
				body: JSON.stringify(data)
			});

			if (res.status === 200) {
                goto('/list');
			} else {
				console.log('Error');
			}

			console.log(res);
		} catch (error) {
			console.error(error);
		}
	}
</script>

<h3>Add Product</h3>

<div class="row mt-5">
	<form on:submit={handleSubmit}>
		<div class="mb-4 col-6">
			<label for="title" class="form-label">Title</label>
			<input
				type="text"
				class="form-control"
				class:is-invalid={errors.title}
				id="title"
				bind:value={formData.title}
			/>
			{#if errors.title}
				<div class="small text-danger position-absolute">
					{errors.title}
				</div>
			{/if}
		</div>

		<div class="mb-4 col-6">
			<label for="description" class="form-label">Description</label>
			<textarea
				class="form-control"
				class:is-invalid={errors.description}
				id="description"
				bind:value={formData.description}
				rows="3"
			/>
			{#if errors.description}
				<div class="small text-danger position-absolute">
					{errors.description}
				</div>
			{/if}
		</div>

		<div class="mb-4 col-6">
			<label for="price" class="form-label">Price</label>
			<input
				type="number"
				class="form-control"
				class:is-invalid={errors.price}
				id="price"
				step="0.01"
				bind:value={formData.price}
			/>
			{#if errors.price}
				<div class="small text-danger position-absolute">
					{errors.price}
				</div>
			{/if}
		</div>

		<div class="mb-4 col-6">
			<label for="discountPercentage" class="form-label">Discount percentage</label>
			<input
				type="number"
				class="form-control"
				class:is-invalid={errors.discountPercentage}
				id="discountPercentage"
				step="0.01"
				bind:value={formData.discountPercentage}
			/>
			{#if errors.discountPercentage}
				<div class="small text-danger position-absolute">
					{errors.discountPercentage}
				</div>
			{/if}
		</div>

		<div class="mb-4 col-6">
			<label for="stock" class="form-label">Stock</label>
			<input
				type="number"
				class="form-control"
				class:is-invalid={errors.stock}
				id="stock"
				bind:value={formData.stock}
			/>
			{#if errors.stock}
				<div class="small text-danger position-absolute">
					{errors.stock}
				</div>
			{/if}
		</div>

		<div class="mb-4 col-6">
			<label for="brand" class="form-label">Brand</label>
			<input
				type="text"
				class="form-control"
				class:is-invalid={errors.brand}
				id="brand"
				bind:value={formData.brand}
			/>
			{#if errors.brand}
				<div class="small text-danger position-absolute">
					{errors.brand}
				</div>
			{/if}
		</div>

		<div class="mb-4 col-6">
			<label for="category" class="form-label">Category</label>
			<input
				type="text"
				class="form-control"
				class:is-invalid={errors.category}
				id="category"
				bind:value={formData.category}
			/>
			{#if errors.category}
				<div class="small text-danger position-absolute">
					{errors.category}
				</div>
			{/if}
		</div>

		<div class="mb-4 col-6">
			<label for="thumbnail" class="form-label">Product image</label>
			<input
				type="file"
				class="form-control"
				class:is-invalid={errors.thumbnail}
				id="thumbnail"
				name="thumbnail"
			/>
			{#if errors.thumbnail}
				<div class="small text-danger position-absolute">
					{errors.thumbnail}
				</div>
			{/if}
		</div>

		<button type="submit" class="btn btn-success mt-3">Save</button>
	</form>
</div>
