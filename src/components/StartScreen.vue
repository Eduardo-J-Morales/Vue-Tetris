<template>
  <div class="start-screen">
    <h1>Welcome to Tetris, {{ playerName }}!</h1>
    <button @click="startGame">Start Game</button>
  </div>
</template>

<script>
export default {
  name: 'StartScreen',
  props: {
    playerName: {
      type: String,
      required: true
    }
  },
  methods: {
    startGame() {
      this.$emit('start-game')
    }
  }
}
</script>

<style scoped>
.start-screen {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
}

button {
  font-size: 2em;
  padding: 10px 20px;
  margin-top: 20px;
}
</style>
