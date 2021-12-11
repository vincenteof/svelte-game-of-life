<script>
  import { range } from 'lodash-es'
  import { onMount } from 'svelte'
  import PubSub from 'pubsub-js'

  export let m = 5
  export let n = 5

  let matrix = createMatrix(m, n)

  console.log('matrix',matrix)

  matrix[2][1] = true
  matrix[2][2] = true
  matrix[2][3] = true

  function createMatrix(innerM, innerN) {
    return range(0, innerM).map(() => Array.from(new Array(innerN), () => false))
  }

  function next(curMatrix, x, y) {
    const poses = [
      [x - 1, y],
      [x - 1, y + 1],
      [x, y + 1],
      [x + 1, y + 1],
      [x + 1, y],
      [x + 1, y - 1],
      [x, y - 1],
      [x - 1, y - 1],
    ]
    const aliveCount = poses.reduce((acc, [posX, posY]) => {
      if (typeof curMatrix[posX] === 'undefined') {
        return acc
      }
      return curMatrix[posX][posY] ? acc + 1 : acc
    }, 0)

    if (curMatrix[x][y]) {
      if (aliveCount === 2 || aliveCount === 3) {
        return curMatrix[x][y]
      }
    } else if (aliveCount === 3) {
      return true
    }
    return false
  }

  function nextGeneration(curMatrix) {
    const newMatrix = createMatrix(curMatrix.length, curMatrix[0].length)
    for (let i = 0; i < curMatrix.length; i++) {
      for (let j = 0; j < curMatrix.length; j++) {
        newMatrix[i][j] = next(curMatrix, i, j)
      }
    }
    return newMatrix
  }

  let timer = null
  function start() {
    if (timer) {
      return
    }
    matrix = nextGeneration(matrix)
    timer = setInterval(() => {
      matrix = nextGeneration(matrix)
    }, 1000);
  }
  function stop() {
    if (timer) {
      clearInterval(timer)
      timer = null
    }
  }

  onMount(() => {
    // todo: is there some more `svelte` way?
    // if we use a module context to exoport an function, we must use a store, then every `matrix` will be changed to `$matrix`
    const startToken = PubSub.subscribe('GAME_START', start)
    const stopToken = PubSub.subscribe('GAME_STOP', stop)
    return () => {
      PubSub.unsubscribe(startToken)
      PubSub.unsubscribe(stopToken)
    }
  })
</script>

<div>
  {#each matrix as _,row}
    <div class="row">
      {#each matrix[row] as _,col}
        <div class="cell" class:alive="{matrix[row][col]}" class:dead="{!matrix[row][col]}"/>
      {/each}
    </div>
  {/each}
</div>

<style>
  .row {
    display: flex;
  }
  .cell {
    border: 3px solid silver;
    margin: -3px -3px 0 0;
    width: 50px;
    height: 50px;
  }
  .alive {
    background-color: #000;
  }
  .dead {
    background-color: #fff;
  }
</style>
