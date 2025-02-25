<template>
    <div>
      <span v-if="!gameStarted">Click Go to test your reaction time!</span><br>
      <span v-if="gameStarted && !readyToStop">Pay attention. Wait for the color change.</span>
      <span v-if="readyToStop">Click stop now!</span>
      <span v-if="errorMessage" class="error">{{ errorMessage }}</span>
  
      <span v-if="reactionTime && !modalVisible" class="span-result">
        Your reaction time was {{ reactionTime }}.
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
  </template>
  
  <script>
  export default {
    name: "GameResult",
    props: ["gameStarted", "readyToStop", "errorMessage", "reactionTime", "highScore"],
    data() {
      return {
        modalVisible: false
      };
    },
    watch: {
      reactionTime(newValue) {
        if (newValue) {
          this.modalVisible = true; 
        }
      }
    },
    methods: {
      closeModal() {
        this.modalVisible = false; 
      }
    }
  };
  </script>
  
  <style>
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
  </style>
  