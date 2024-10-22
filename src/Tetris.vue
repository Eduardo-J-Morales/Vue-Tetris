<template>
  <div class="tetris-container">
    <h1>Tetris</h1>
    <div class="game-board">
      <!-- Game board will be added here later -->
    </div>
    <div class="player-name">Player: {{ playerName }}</div>
    <StartScreen 
      v-if="!gameStarted" 
      :playerName="playerName" 
      @start-game="startGame"
    />
  </div>
</template>

<script>
import StartScreen from './components/StartScreen.vue'

export default {
  name: 'Tetris',
  components: {
    StartScreen
  },
  data() {
    return {
      playerName: 'Your Name', // Replace 'Your Name' with your actual name
      gameStarted: false
      // We'll add more data properties as we progress
    }
  },
  methods: {
    startGame() {
      this.gameStarted = true
      // We'll add game initialization logic here later
    }
  }
}
</script>

<style scoped>
.tetris-container {
  text-align: center;
}

.player-name {
  margin-top: 20px;
  font-size: 18px;
}
</style>
