<template>
  <div class="tetromino" :style="style">
    <div v-for="(row, rowIndex) in shape" :key="rowIndex" class="tetromino-row">
      <div v-for="(cell, cellIndex) in row" :key="cellIndex" 
           class="tetromino-cell" :class="{ filled: cell === 1 }">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Tetromino',
  props: {
    shape: {
      type: Array,
      required: true
    },
    color: {
      type: String,
      default: '#000'
    },
    position: {
      type: Object,
      required: true
    }
  },
  computed: {
    style() {
      return {
        '--tetromino-color': this.color,
        left: `${this.position.x * 30}px`,
        top: `${this.position.y * 30}px`
      }
    }
  }
}
</script>

<style scoped>
.tetromino {
  position: absolute;
}

.tetromino-cell {
  width: 30px;
  height: 30px;
  box-sizing: border-box;
}

.tetromino-cell.filled {
  background-color: var(--tetromino-color);
  border: 1px solid rgba(0, 0, 0, 0.3);
}
</style>
