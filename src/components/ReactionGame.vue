<template>
  <div class="canvas" :class="{ 'active': readyToStop }">
    <h2>Reaction Time Game</h2>
    <button 
      @click="handleButtonClick"
      :class="gameStarted ? 'stop' : 'go'"
      class="action-btn"
    >
      {{ gameStarted ? 'Stop' : 'Go' }}
    </button>

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
import GameResult from "./GameResult.vue";

export default {
  components: { GameResult },
  name: "ReactionGame",
  data() {
    return {
      gameStarted: false,
      readyToStop: false,
      startTime: 0,
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

      this.timer = setTimeout(() => {
        this.readyToStop = true;
        this.startTime = performance.now();
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

      const currentReactionTime = Math.round(performance.now() - this.startTime);
      this.reactionTime = currentReactionTime;
      this.gameStarted = false;
      this.readyToStop = false;

      if (this.highScore === null || currentReactionTime < parseInt(this.highScore)) {
        this.highScore = currentReactionTime;
      }
    }
  }
};
</script>

<style>
  body {
    background: #ffffff;
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  h2{
      color: rgb(145, 10, 75);
      font-weight: 900;
  }

  .canvas {
    border: 2px solid black;
    border-radius: 20px;
    height: 60vh;
    width: 90vw;
    max-width: 700px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;
    background-color: skyblue;
    transition: background-color 0.3s ease;
  }
  .canvas.active {
    background-color:  rgb(208, 97, 210);
  }
  .action-btn {
    height: 15%;
    width: 80%;
    max-width: 400px;
    border: none;
    font-size: 1.5rem;
    border-radius: 20px;
    color: white;
  }
  .go {
    background-color: rgb(30, 196, 30);
  }
  .stop {
    background-color: rgb(226, 19, 19);
  }
  .result {
    height: 25%;
    width: 80%;
    max-width: 400px;
    color: black;
    border: 1px solid;
    border-radius: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: 1.2rem;
    text-align: center;
    background-color: aliceblue;
    padding: 10px;
  }
  .span-result {
    font-weight: bold;
  }
  .error {
    color: red;
    font-weight: bold;
  }
  @media (max-width: 600px) {
    .canvas {
      height: 70vh;
    }
    .action-btn {
      font-size: 1.2rem;
    }
    .result {
      font-size: 0.9rem;
    }
  }
</style>