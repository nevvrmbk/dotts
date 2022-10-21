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

<section class="flex flex-col justify-evenly items-center container mx-auto py-2 h-screen">
	<h2>User</h2>
	{#await getUser()}
		<p>Getting user...</p>
	{:then { results }}
		{#each results as { email, picture: { large }, name: { first, last }, dob: { date }, phone }}
			{@const dob = new Date(date).getFullYear()}
			{@const year = new Date().getFullYear()}
			<figure class="flex flex-col justify-around items-center">
				<img class="rounded-full" src={large} alt="{first}-{last}" />

				<figcaption class="flex flex-col justify-around items-center">
					<p class="font-medium text-lg text-blue-600">{first} {last}</p>
					<a href="mailto:{email}">{email}</a>
					<p>{dob}</p>
					<p>{year - dob}</p>
					<a href="tel:+{phone}">{phone}</a>
				</figcaption>
			</figure>
		{:else}
			<p>No user found.</p>
		{/each}
	{:catch error}
		<p>{error.message}</p>
	{/await}
</section>
