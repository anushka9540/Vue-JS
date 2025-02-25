<template>
<button 
      @click="$emit('button-click')"
      :class="gameStarted ? 'stop' : 'go'"
      class="action-btn"
    >
      {{ gameStarted ? 'Stop' : 'Go' }}
    </button>
</template>

<script>
    export default{
        name:'GameButton',
        props: {
            gameStarted: Boolean
        }
    }
</script>

<style>
.action-btn {
    height: 60px;
    width: 80%;
    max-width: 450px;
    border: none;
    font-size: 1.6rem;
    font-weight: bold;
    border-radius: 25px;
    color: white;
    cursor: pointer;
    transition: all 0.2s ease-in-out;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
  }
  .go {
  background: linear-gradient(to right, #38ef7d, #11998e);
}

.stop {
  background: linear-gradient(to right, #ff416c, #ff4b2b);
}
.action-btn:hover {
  transform: scale(1.05);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.25);
}

.action-btn:active {
  transform: scale(0.97);
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.2);
}

</style>