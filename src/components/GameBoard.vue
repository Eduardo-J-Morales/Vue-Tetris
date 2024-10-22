<template>
  <div class="game-board" tabindex="0" @keydown="handleKeydown" ref="gameBoard">
    <div v-for="row in rows" :key="row" class="row">
      <div v-for="col in cols" :key="col" class="cell" :style="getCellStyle(row-1, col-1)"></div>
    </div>
    <Tetromino 
      :shape="currentPiece.shape" 
      :color="currentPiece.color"
      :position="currentPiece.position"
    />
    <div class="score">Score: {{ score }}</div>
  </div>
</template>

<script>
import Tetromino from './Tetromino.vue'

// Define shapes and colors for Tetromino pieces
// Definir formas e cores para as peças do Tetromino
// Definieren Sie Formen und Farben für Tetromino-Teile
const SHAPES = [
  [[1, 1, 1, 1]],
  [[1, 1], [1, 1]],
  [[1, 1, 1], [0, 1, 0]],
  [[1, 1, 1], [1, 0, 0]],
  [[1, 1, 1], [0, 0, 1]],
  [[1, 1, 0], [0, 1, 1]],
  [[0, 1, 1], [1, 1, 0]]
];

const COLORS = ['#FF0000', '#00FF00', '#0000FF', '#FFFF00', '#FF00FF', '#00FFFF', '#FFA500'];

export default {
  name: 'GameBoard',
  components: {
    Tetromino
  },
  data() {
    return {
      // Initialize game state
      // Inicializar o estado do jogo
      // Initialisieren Sie den Spielzustand
      rows: 20,
      cols: 10,
      board: Array(20).fill().map(() => Array(10).fill(0)),
      currentPiece: this.generateNewPiece(),
      gameState: 'active',
      score: 0,
      dropInterval: null
    }
  },
  methods: {
    // Handle keyboard input for piece movement and rotation
    // Lidar com entrada do teclado para movimento e rotação da peça
    // Behandeln Sie Tastatureingaben für Teilebewegung und -rotation
    handleKeydown(event) {
      if (this.gameState !== 'active') return;

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
        case 'ArrowUp':
          this.rotatePiece();
          break;
      }
    },

    // Move the current piece if the move is valid
    // Mover a peça atual se o movimento for válido
    // Bewegen Sie das aktuelle Teil, wenn der Zug gültig ist
    movePiece(dx, dy) {
      const newX = this.currentPiece.position.x + dx;
      const newY = this.currentPiece.position.y + dy;
      
      if (this.isValidMove(newX, newY, this.currentPiece.shape)) {
        this.currentPiece.position.x = newX;
        this.currentPiece.position.y = newY;
      } else if (dy > 0) {
        this.lockPiece();
        this.clearRows();
        this.currentPiece = this.generateNewPiece();
        if (!this.isValidMove(this.currentPiece.position.x, this.currentPiece.position.y, this.currentPiece.shape)) {
          this.gameState = 'over';
          clearInterval(this.dropInterval);
        }
      }
    },

    // Rotate the current piece and apply wall kicks if necessary
    // Girar a peça atual e aplicar "wall kicks" se necessário
    // Drehen Sie das aktuelle Teil und wenden Sie bei Bedarf Wandtritte an
    rotatePiece() {
      const rotated = [];
      for (let i = 0; i < this.currentPiece.shape[0].length; i++) {
        const row = [];
        for (let j = this.currentPiece.shape.length - 1; j >= 0; j--) {
          row.push(this.currentPiece.shape[j][i]);
        }
        rotated.push(row);
      }
      
      // Try to rotate, if not possible, try wall kicks
      if (this.isValidMove(this.currentPiece.position.x, this.currentPiece.position.y, rotated)) {
        this.currentPiece.shape = rotated;
      } else {
        // Try wall kicks: right, left, up
        const kicks = [
          { x: 1, y: 0 },
          { x: -1, y: 0 },
          { x: 0, y: -1 },
          { x: 2, y: 0 },
          { x: -2, y: 0 }
        ];
        
        for (const kick of kicks) {
          const newX = this.currentPiece.position.x + kick.x;
          const newY = this.currentPiece.position.y + kick.y;
          if (this.isValidMove(newX, newY, rotated)) {
            this.currentPiece.shape = rotated;
            this.currentPiece.position.x = newX;
            this.currentPiece.position.y = newY;
            break;
          }
        }
      }
    },

    // Check if the move is valid (within bounds and not colliding)
    // Verificar se o movimento é válido (dentro dos limites e sem colisão)
    // Überprüfen Sie, ob der Zug gültig ist (innerhalb der Grenzen und nicht kollidierend)
    isValidMove(x, y, shape) {
      for (let row = 0; row < shape.length; row++) {
        for (let col = 0; col < shape[row].length; col++) {
          if (shape[row][col]) {
            const newX = x + col;
            const newY = y + row;
            
            if (newX < 0 || newX >= this.cols || newY >= this.rows) {
              return false;
            }
            
            if (newY >= 0 && this.board[newY][newX]) {
              return false;
            }
          }
        }
      }
      return true;
    },

    // Lock the current piece in place on the board
    // Fixar a peça atual no tabuleiro
    // Sperren Sie das aktuelle Teil auf dem Brett
    lockPiece() {
      for (let row = 0; row < this.currentPiece.shape.length; row++) {
        for (let col = 0; col < this.currentPiece.shape[row].length; col++) {
          if (this.currentPiece.shape[row][col]) {
            const boardRow = this.currentPiece.position.y + row;
            const boardCol = this.currentPiece.position.x + col;
            if (boardRow >= 0 && boardRow < this.rows && boardCol >= 0 && boardCol < this.cols) {
              this.board[boardRow][boardCol] = this.currentPiece.color;
            }
          }
        }
      }
    },

    // Clear completed rows and update the score
    // Limpar linhas completas e atualizar a pontuação
    // Löschen Sie vollständige Zeilen und aktualisieren Sie die Punktzahl
    clearRows() {
      let rowsCleared = 0;
      for (let row = this.rows - 1; row >= 0; row--) {
        if (this.board[row].every(cell => cell !== 0)) {
          this.board.splice(row, 1);
          this.board.unshift(Array(this.cols).fill(0));
          rowsCleared++;
          row++; // Check the same row again
        }
      }
      if (rowsCleared > 0) {
        this.score += [40, 100, 300, 1200][rowsCleared - 1];
      }
    },

    // Get the style for a cell based on its state
    // Obter o estilo para uma célula com base em seu estado
    // Holen Sie sich den Stil für eine Zelle basierend auf ihrem Zustand
    getCellStyle(row, col) {
      return {
        backgroundColor: this.board[row][col] || 'transparent'
      }
    },

    // Generate a new random Tetromino piece
    // Gerar uma nova peça aleatória do Tetromino
    // Generieren Sie ein neues zufälliges Tetromino-Teil
    generateNewPiece() {
      const shapeIndex = Math.floor(Math.random() * SHAPES.length);
      return {
        shape: SHAPES[shapeIndex],
        color: COLORS[shapeIndex],
        position: { x: Math.floor(this.cols / 2) - Math.floor(SHAPES[shapeIndex][0].length / 2), y: 0 }
      };
    },

    // Start the game loop
    // Iniciar o loop do jogo
    // Starten Sie die Spielschleife
    startGame() {
      this.dropInterval = setInterval(() => {
        this.movePiece(0, 1);
      }, 1000);
    }
  },
  mounted() {
    // Set focus on the game board and start the game when component is mounted
    // Definir foco no tabuleiro do jogo e iniciar o jogo quando o componente é montado
    // Setzen Sie den Fokus auf das Spielbrett und starten Sie das Spiel, wenn die Komponente montiert ist
    this.$refs.gameBoard.focus();
    this.startGame();
  },
  beforeUnmount() {
    // Clear the game loop interval when component is unmounted
    // Limpar o intervalo do loop do jogo quando o componente é desmontado
    // Löschen Sie das Spielschleifen-Intervall, wenn die Komponente abgebaut wird
    clearInterval(this.dropInterval);
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

.cell.filled {
  background-color: #999;
}

.score {
  position: absolute;
  top: -30px;
  left: 0;
  font-size: 18px;
  font-weight: bold;
}
</style>
