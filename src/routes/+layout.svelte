<script>
	// import First from '../lib/components/First.svelte';
	import Header from '../lib/components/Design/Header.svelte';
</script>

<!-- <First /> -->
<div class="app">
	<Header title={'Users Directory'} />

	<div class="container">
		<slot />
	</div>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		height: 100vh;
	}

	.container {
		margin-top: 32px;
	}
</style>
