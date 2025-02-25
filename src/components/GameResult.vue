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
    padding: 10px;
  }
  .modal-content {
    background: white;
    padding: 20px;
    border-radius: 15px;
    text-align: center;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
    width: 90%;
    max-width: 400px;
    font-size: 1.2rem;
  }
.modal-content h3 {
  font-size: 1.6rem;
  color: #444;
}

.modal-content p {
  font-size: 1rem;
  margin: 10px 0;
}
  .modal-content button {
    margin-top: 15px;
    padding: 12px 20px;
    border: none;
    background: #28a745;
    color: white;
    font-size: 1rem;
    border-radius: 8px;
    cursor: pointer;
    transition: 0.3s;
  }
  .high-score {
    display: block;
    margin-top: 10px;
    font-size: 1.3rem;
    font-weight: bold;
    color: darkblue;
  }

  .result {
    height: auto;
    width: 85%;
    max-width: 450px;
    color: black;
    border: 2px solid #444;
    border-radius: 25px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: 1.4rem;
    text-align: center;
    background-color: aliceblue;
    padding: 15px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
  }
  .modal-content button:hover {
  background: #218838;
}
  .span-result {
    font-weight: bold;
  }
  .error {
    color: red;
    font-weight: bold;
    font-size: 1.3rem;
  }
  </style>
  