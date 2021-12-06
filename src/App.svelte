<script>
	import Game from './components/Game.svelte'
</script>

<main>
	<Game />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		width: 100%;
		height: 100%;
		margin: 0 auto;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>