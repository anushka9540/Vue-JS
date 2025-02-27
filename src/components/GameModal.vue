<template>
    <div v-if="show" class="modal">
        <div class="modal-content">
            <h3>Reaction Time Result</h3>
            {{ result }}
            <p v-if="highScore">
                High Score: <strong>{{ highScore }}s</strong>
            </p>
            <button v-on:click="$emit('close')">OK</button>
        </div>
    </div>
</template>
  
  <script>
export default {
  name: 'GameModal',
  props: {
    show: Boolean,
    result: String,
    highScore: String
  }
};
  </script>
  
  <style scoped>
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
</style>