<template>
  <!-- Render the Tetromino piece -->
  <!-- Renderizar a peça do Tetromino -->
  <!-- Rendern Sie das Tetromino-Teil -->
  <div class="tetromino" :style="tetrominoStyle">
    <div v-for="(row, rowIndex) in shape" :key="rowIndex" class="tetromino-row">
      <div v-for="(cell, cellIndex) in row" :key="cellIndex" 
           class="tetromino-cell" :class="{ filled: cell === 1 }" 
           :style="{ backgroundColor: cell === 1 ? color : 'transparent' }">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Tetromino',
  props: {
    // Define props for the Tetromino component
    // Definir props para o componente Tetromino
    // Definieren Sie Props für die Tetromino-Komponente
    shape: {
      type: Array,
      required: true
    },
    color: {
      type: String,
      required: true
    },
    position: {
      type: Object,
      required: true
    }
  },
  computed: {
    // Calculate the style for positioning the Tetromino
    // Calcular o estilo para posicionar o Tetromino
    // Berechnen Sie den Stil für die Positionierung des Tetrominos
    tetrominoStyle() {
      return {
        position: 'absolute',
        left: `${this.position.x * 30}px`,
        top: `${this.position.y * 30}px`
      }
    }
  }
}
</script>

<style scoped>
/* Style the Tetromino and its cells */
/* Estilizar o Tetromino e suas células */
/* Gestalten Sie das Tetromino und seine Zellen */
.tetromino {
  position: absolute;
}
.tetromino-row {
  display: flex;
}
.tetromino-cell {
  width: 30px;
  height: 30px;
  box-sizing: border-box;
  border: 1px solid rgba(0, 0, 0, 0.1);
}
.tetromino-cell.filled {
  border: 1px solid rgba(0, 0, 0, 0.3);
}
</style>
