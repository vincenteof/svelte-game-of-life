<script>
  import {createEventDispatcher} from 'svelte'
  import PubSub from 'pubsub-js'

  
  function start() {
    PubSub.publish('GAME_START', null)
  }

  function stop() {
    PubSub.publish('GAME_STOP', null)
  }

  let m
  let n
  
  const dispatch = createEventDispatcher()

  function setGameSize() {
    dispatch('gameSizeChange', [m, n])
  }

</script>

<div>
  <button on:click={start}>start</button>
  <button on:click={stop}>stop</button>
  <div>
    The matrix is<input type="number" bind:value={m} /> * <input type="number" bind:value={n}/>
    <button on:click={setGameSize}>set</button>
  </div>
</div>