
<script>
	import { onMount } from 'svelte';

	let xxx = [];

	onMount(async () => {
		const res = await fetch(`./aa1.pgn`);
		xxx = await res.text();
	});
</script>
<main>

	<h1>Hello {xxx}!</h1>

</main>

<style>



	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>