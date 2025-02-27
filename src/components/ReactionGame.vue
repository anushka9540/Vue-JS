<template>
  <div class="canvas" :class="{ 'active': startTime !== null }">
    <h1>Reaction Time Game</h1>
    <GameButton :gameStarted="gameStarted" v-on:click="toggleGameState()" />
  
    <div class="result">
      <GameResult :gameStarted="gameStarted" :reactionTime="reactionTime" :highScore="highScore || null" />
  
      <GameModal v-if="showModal" :show="showModal" :reactionTime="reactionTime" :highScore="highScore"
        @close="closeModal" />
    </div>
  </div>
</template>

<script>
import GameButton from './GameButton.vue';
import GameModal from './GameModal.vue';
import GameResult from "./GameResult.vue";

export default {
  components: { GameResult, GameButton, GameModal },
  name: "ReactionGame",
  data() {
    return {
      gameStarted: false,
      startTime: null,
      reactionTime: null,
      highScore: null,
      reactionTimer: null,
      showModal: false
    };
  },
  methods: {
    toggleGameState() {
      this.gameStarted ? this.stopGame() : this.startGame();
    },
    startGame() {
      this.gameStarted = true;
      this.reactionTime = null;
      this.startTime = null;
      this.showModal = false;
      this.reactionTimer = setTimeout(() => {
        this.startTime = Date.now();
      }, 5000);
    },
    stopGame() {
      if (!this.startTime) {
        this.reactionTime = "too soon";
        this.gameStarted = false;
        clearTimeout(this.reactionTimer);
        return;
      }
      const rawTime = (Date.now() - this.startTime) / 1000;
      this.reactionTime = rawTime.toFixed(3);
      this.showModal = true;
      this.gameStarted = false;
      this.startTime = null;
      if (this.highScore === null || parseFloat(this.reactionTime) < this.highScore) {
        this.highScore = this.reactionTime;
      }
    },
    closeModal() {
      this.showModal = false;
    }
  }
};
</script>


<style>
body {
  background-color: skyblue;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

h1 {
  color: rgb(145, 10, 75);
  font-weight: 900;
}

.canvas {
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
  transition: background-color 0.3s ease;
}

.canvas.active {
  background-color: rgb(208, 97, 210);
}

.high-score {
  display: block;
  margin-top: 10px;
  font-size: 1.2rem;
  font-weight: bold;
  color: darkblue;
}

@media (max-width: 600px) {
  .action-btn {
    font-size: 1.2rem;
  }

  .result {
    font-size: 0.9rem;
  }
}
</style>