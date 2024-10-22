<template>
  <div class="game-board" tabindex="0" @keydown="handleKeydown" ref="gameBoard">
    <div v-for="row in rows" :key="row" class="row">
      <div v-for="col in cols" :key="col" class="cell"></div>
    </div>
    <Tetromino 
      :shape="currentPiece.shape" 
      :color="currentPiece.color"
      :position="currentPiece.position"
    />
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
        color: '#FF0000',
        position: { x: 3, y: 0 }
      }
    }
  },
  methods: {
    handleKeydown(event) {
      switch(event.key) {
        case 'ArrowLeft':
          this.movePiece(-1, 0);
          break;
        case 'ArrowRight':
          this.movePiece(1, 0);
          break;
        case 'ArrowDown':
          this.movePiece(0, 1);
          break;
      }
    },
    movePiece(dx, dy) {
      const newX = this.currentPiece.position.x + dx;
      const newY = this.currentPiece.position.y + dy;
      
      if (this.isValidMove(newX, newY)) {
        this.currentPiece.position.x = newX;
        this.currentPiece.position.y = newY;
      }
    },
    isValidMove(x, y) {
      // Basic boundary check
      return x >= 0 && x + this.currentPiece.shape[0].length <= this.cols &&
             y >= 0 && y + this.currentPiece.shape.length <= this.rows;
    }
  },
  mounted() {
    this.$refs.gameBoard.focus();
  }
}
</script>

<style scoped>
.game-board {
  display: inline-block;
  border: 2px solid #333;
  background-color: #f0f0f0;
  position: relative;
  outline: none;
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
