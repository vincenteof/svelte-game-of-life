<script>
	import Game from './components/Game.svelte'
	import Opertions from './components/Operations.svelte'
</script>

<main>
	<Opertions />
	<Game m={8} n={8} />
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
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>