<script>
	export let user = {
		id: null,
		name: '',
		username: '',
		email: '',
		address: {
			street: '',
			suite: '',
			city: '',
			zipcode: '',
			geo: {
				lat: '',
				lng: ''
			}
		},
		phone: '',
		website: '',
		company: {
			name: '',
			catchPhrase: '',
			bs: ''
		},
		photo: ''
	};
</script>

<div class="card mb-4">
	<img src={user.photo} class="card-img-top" alt={user.name} />
	<div class="card-body">
		<h5 class="card-title">{user.name}</h5>
		<p class="card-text">
			{user.address?.street}, {user.address?.suite}, {user.address?.zipcode}, {user.address?.city}
		</p>
	</div>
	<ul class="list-group list-group-flush">
		<li class="list-group-item"><b>Username:</b> @{user.username}</li>
		<li class="list-group-item"><b>Email:</b> {user.email}</li>
		<li class="list-group-item"><b>Phone:</b> {user.phone}</li>
		<li class="list-group-item"><b>Company:</b> {user.company?.name}</li>
	</ul>
</div>

<style>
	.card {
		width: 400px;
		padding: 12px;
	}
</style>
