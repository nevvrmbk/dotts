<script lang="ts">
	async function getUser() {
		const response = await fetch('https://randomuser.me/api/');
		const data = await response.json();

		if (response.ok && response.status == 200) {
			return data;
		} else {
			throw new Error(data);
		}
	}
</script>

<section class="flex flex-col justify-around items-center border border-black border-solid rounded-md container mx-auto py-2 h-screen">
	<h2>User</h2>
	{#await getUser()}
		<p>Getting user...</p>
	{:then { results }}
		{#each results as { email, picture: { large }, name: { first, last } }}
			<figure>
				<img class="rounded-full" src={large} alt="{first}-{last}" />

				<figcaption>
					<p>{first} {last}</p>
					<p>{email}</p>
				</figcaption>
			</figure>
		{:else}
			<p>No user found.</p>
		{/each}
	{:catch error}
		<p>{error.message}</p>
	{/await}
</section>
