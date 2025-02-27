<template>
  <div class="canvas" :class="{ 'active': startTime !== null }">
    <h1>Reaction Time Game</h1>
    <GameButton :gameStarted="gameStarted" v-on:click="toggleGameState()" />
  
    <div class="result">
      <GameResult :gameStarted="gameStarted" :reactionTime="lastReactionTime" :highScore="highScore || null" />
  
      <div v-if="reactionTime !== null && reactionTime !== 'too soon'" class="modal">
        <div class="modal-content">
          <h3>Reaction Time Result</h3>
          <p>Your reaction time was <strong>{{ reactionTime }}s</strong></p>
          <p v-if="highScore">
            High Score: <strong>{{ highScore }}s</strong>
          </p>
          <button v-on:click="closeModal">OK</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import GameButton from './GameButton.vue';
import GameResult from "./GameResult.vue";

export default {
  components: { GameResult, GameButton },
  name: "ReactionGame",
  data() {
    return {
      gameStarted: false,
      startTime: null,
      reactionTime: null,
      highScore: null,
      reactionTimer: null,
      lastReactionTime: null
    };
  },
  methods: {
    toggleGameState() {
      this.gameStarted ? this.stopGame() : this.startGame();
    },
    startGame() {
      this.gameStarted = true;
      this.reactionTime = null;
      this.lastReactionTime = null;
      this.startTime = null;
      this.reactionTimer = setTimeout(() => {
        this.startTime = Date.now();
      }, 5000);
    },
    stopGame() {
      if (!this.startTime) {
        this.reactionTime = "too soon";
        this.lastReactionTime = "too soon";
        this.gameStarted = false;
        clearTimeout(this.reactionTimer);
        return;
      }
      const rawTime = (Date.now() - this.startTime) / 1000;
      this.reactionTime = rawTime.toFixed(3);
      this.lastReactionTime = this.reactionTime;
      this.gameStarted = false;
      this.startTime = null;
      if (this.highScore === null || parseFloat(this.reactionTime) < this.highScore) {
        this.highScore = this.reactionTime;
      }
    },
    closeModal() {
      this.reactionTime = null;
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

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
}

.modal-content button {
  margin-top: 10px;
  padding: 10px 20px;
  border: none;
  background: rgb(30, 196, 30);
  color: white;
  border-radius: 5px;
  cursor: pointer;
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
