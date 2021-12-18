<script>
	import Game from './components/Game.svelte'
	import Opertions from './components/Operations.svelte'

	// todo: can we use 2-way binding?
	let m = 8
	let n = 8

	function onGameSizeChange(e) {
		console.log(e)
		const [innerM, innerN] = e.detail
		m = innerM
		n = innerN
	}
</script>

<main>
	<Opertions on:gameSizeChange={onGameSizeChange} />
	<Game m={m} n={n} />
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