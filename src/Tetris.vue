<template>
  <div class="tetris-container">
    <h1>Tetris</h1>
    <StartScreen 
      v-if="!gameStarted" 
      :playerName="playerName" 
      @start-game="startGame"
    />
    <GameBoard v-if="gameStarted" />
    <div class="player-name">Player: {{ playerName }}</div>
  </div>
</template>

<script>
import StartScreen from './components/StartScreen.vue'
import GameBoard from './components/GameBoard.vue'

export default {
  name: 'Tetris',
  components: {
    StartScreen,
    GameBoard
  },
  data() {
    return {
      playerName: 'Eduardo Morales', // Replace 'Your Name' with your actual name
      gameStarted: false
    }
  },
  methods: {
    startGame() {
      this.gameStarted = true
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
