<template>
  <div class="game-board">
    <div v-for="row in rows" :key="row" class="row">
      <div v-for="col in cols" :key="col" class="cell"></div>
    </div>
    <Tetromino :shape="currentPiece.shape" :color="currentPiece.color" />
  </div>
</template>

<script>
import Tetromino from './Tetromino.vue'

export default {
  name: 'GameBoard',
  components: {
    Tetromino
  },
  data() {
    return {
      rows: 20,
      cols: 10,
      currentPiece: {
        shape: [
          [0, 1, 0],
          [1, 1, 1],
          [0, 0, 0]
        ],
        color: '#FF0000'
      }
    }
  }
}
</script>

<style scoped>
.game-board {
  display: inline-block;
  border: 2px solid #333;
  background-color: #f0f0f0;
  position: relative;
}

.row {
  display: flex;
}

.cell {
  width: 30px;
  height: 30px;
  border: 1px solid #ccc;
  box-sizing: border-box;
}
</style>
