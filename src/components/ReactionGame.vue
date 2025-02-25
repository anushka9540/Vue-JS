<template>
  <div class="canvas" :class="{ 'active': readyToStop }">
    <h1>Reaction Time Game</h1>
    <GameButton 
      :gameStarted="gameStarted"
      @btn-text-changed="handleButtonClick"
    />

    <div class="result">
      <GameResult
        :gameStarted="gameStarted"
      />
       
      <span v-if="reactionTime && !modalVisible" class="span-result">
        Your reaction time was {{ rawReactionTime }}.
      </span>

      <span v-if="highScore" class="high-score">High Score: {{ highScore }}</span>

      <div v-if="modalVisible" class="modal">
        <div class="modal-content">
          <h3>Reaction Time Result</h3>
          <p>Your reaction time was <strong>{{ reactionTime }}</strong></p>
          <p v-if="highScore">High Score : <strong>{{ highScore }}</strong></p>
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
      readyToStop: false,
      startTime: null, 
      reactionTime: null,
      rawReactionTime: null,
      highScore: null, 
      timer: null,
      reactionDelay: 5000,
      modalVisible: false, 
    };
  },
  methods: {
    handleButtonClick() {
      if (!this.gameStarted) {
        this.startGame();
      } else {
        this.stopGame();
      }
    },
    startGame() {
      this.gameStarted = true;
      this.reactionTime = null;
      this.rawReactionTime = null; 
      this.resultMessage = "";
      this.readyToStop = false;
      this.startTime = null; 

      this.timer = setTimeout(() => {
        this.readyToStop = true;
        this.startTime = Date.now(); 
      }, this.reactionDelay);
    },
    stopGame() {
      if (!this.readyToStop || !this.startTime) {
        this.reactionTime = null;
        this.rawReactionTime = null;
        this.gameStarted = false;
        clearTimeout(this.timer);
        return;
      }

      const endTime = Date.now();
      this.rawReactionTime = endTime - this.startTime;

      let seconds = Math.floor(this.rawReactionTime / 1000);
      let milliseconds = this.rawReactionTime % 1000;

      this.reactionTime = `${seconds}.${milliseconds.toString().padStart(3, '0')}s`;
      
      this.rawReactionTime = this.reactionTime;

      this.gameStarted = false;
      this.readyToStop = false;

      if (this.highScore === null || this.reactionTime < this.highScore) {
        this.highScore = this.reactionTime;
      }

      this.modalVisible = true;
    },
    closeModal() {
      this.modalVisible = false;
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

  h1{
      color: rgb(145, 10, 75);
      font-weight: 900;
  }

  .canvas {
    margin-left: 0;
    height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;
    transition: background-color 0.3s ease;
  }
  .canvas.active {
    background-color:  rgb(208, 97, 210);
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
