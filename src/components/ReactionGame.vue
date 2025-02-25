<template>
  <div class="canvas" :class="{ 'active': readyToStop }">
    <h2>Reaction Time Game</h2>
    <GameButton 
      :gameStarted="gameStarted"
      @button-click="handleButtonClick"
    />

    <div class="result">
      <GameResult
        :gameStarted="gameStarted"
        :readyToStop="readyToStop"
        :errorMessage="resultMessage"
        :reactionTime="reactionTime"
        :highScore="highScore"
      />
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
      counterInterval: null,
      reactionTime: null,
      highScore: null, 
      timer: null,
      resultMessage: "",
      reactionDelay: 3000 
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
      this.resultMessage = "";
      this.readyToStop = false;
      this.counter = 0;

      this.timer = setTimeout(() => {
        this.readyToStop = true;
        this.counterInterval = setInterval(() => {
          this.counter++; 
        }, 1);
      }, this.reactionDelay);
    },
    stopGame() {
      if (!this.readyToStop) {
        this.resultMessage = "Too soon!";
        this.reactionTime = null;
        this.gameStarted = false;
        clearTimeout(this.timer);
        return;
      }

      clearInterval(this.counterInterval); 

      let seconds = Math.floor(this.counter / 1000);
      let milliseconds = this.counter % 1000;

      this.reactionTime = seconds + "." + (milliseconds < 100 ? (milliseconds < 10 ? "00" : "0") : "") + milliseconds + "s";
      
      this.gameStarted = false;
      this.readyToStop = false;

      if (this.highScore === null || this.reactionTime < this.highScore) {
        this.highScore = this.reactionTime;
      }
    }
  }
};
</script>


<style>
  body {
    background: linear-gradient(to right, #74ebd5, #9face6);
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    font-family: 'Arial', sans-serif;
  }

  h2{
      color: #910a4b;
      font-weight: 900;
      font-size: 2rem;
      text-transform: uppercase;
      letter-spacing: 2px;
      text-shadow: 2px 2px 10px rgba(0, 0, 0, 0.2);
      text-align: center;
  }

  .canvas {
    border: 4px solid #333;
    border-radius: 25px;
    height: 70vh;
    width: 90vw;
    max-width: 800px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;
    background-color: #ffffff;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
    padding: 20px;
    transition: background-color 0.3s ease, transform 0.2s;
  }
  .canvas.active {
    background-color: #d162d1;
    transform: scale(1.02);
  }
  @media (max-width: 600px) {
  .canvas {
    height: 85vh;
    width: 95vw;
    padding: 10px;
  }

  h2 {
    font-size: 1.6rem;
  }

  .action-btn {
    width: 90%;
    height: 55px;
    font-size: 1.4rem;
  }

  .result {
    font-size: 1.2rem;
    width: 90%;
  }

  .modal-content {
    width: 80%;
    font-size: 1rem;
    padding: 5px;
    margin-right: 55px;
  }

  .modal-content button {
    font-size: 1rem;
    padding: 10px 18px;
  }
}
</style>