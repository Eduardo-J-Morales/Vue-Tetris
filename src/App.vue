<template>
  <div class="tetris">
    <div class="game-board">
      <div v-for="(row, y) in board" :key="y" class="board-row">
        <div v-for="(cell, x) in row" :key="x" :class="['cell', {filled: cell}]"></div>
      </div>
    </div>
    <div class="game-info">
      <p>Score: <span class="score">{{ score }}</span></p>
      <p>Level: <span class="level">{{ level }}</span></p>
      <button @click="startGame">Start Game</button>
    </div>
  </div>
</template>

<script>
export default {
  const BOARD_SIZE = 10;
  const BOARD_HEIGHT = 20
  name: 'Tetris',
  data() {
    return {
      score: 0,
      level: 1,
      board: [],
    }
  },
  methods: {
    startGame() {

    }
  }
}
</script>

<style scoped>
.tetris {
  display: flex;
  justify-content: center;
  align-items: center;
}

.game-board {
  
}

.game-info {

}
</style>