<script>
  import PubSub from 'pubsub-js'

  function start() {
    PubSub.publish('GAME_START', null)
  }

  function stop() {
    PubSub.publish('GAME_STOP', null)
  }
</script>

<div>
  <button on:click={start}>start</button>
  <button on:click={stop}>stop</button>
</div>