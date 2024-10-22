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
      currentPiece: null,
      gameInterval: null,
    }
  },
  methods: {
    startGame() {
      
    }
  }
    initBoard() {
      this.board = Array(BOARD_HEIGHT).fill().map(() => Array(BOARD_SIZE).fill(0));
    },

    spawnPiece() {
      this.currentPiece = {
        shape: [[1,1,1,1]],
        x: Math.floor(BOARD_WIDTH / 2) - 2,
        y: 0
      };
    },
    gameLoop() {
      if (this.canMovePiece(0, 1)) {
        this.movePiece(0, 1);
      } else {
        this.placePiece();
        this.clearLines();
        this.spawnPiece();
        if (!this.canMovePiece(0, 0)) {
          this.gameOver();
        }
      }
    },
    canMovePiece(dx, dy) {

    },
    movePiece(dx, dy) {
      this.currentPiece.x += dx;
      this.currentPiece.y += dy;
    },
    placePiece() {

    },
    clearLines() {

    },
    gameOver() {
      clearInterval(this.gameInterval);
      alert('Game Over!')
    }

}
</script>

<style scoped>

</style>