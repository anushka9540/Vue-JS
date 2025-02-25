<template>
    <button @click="$emit('btn-text-changed')" :class="gameStarted ? 'stop' : 'go'" class="action-btn">
        {{ gameStarted ? 'Stop' : 'Go' }}
    </button>
</template>

<script>
export default {
  name: 'GameButton',
  props: {
    gameStarted: Boolean
  }
};
</script>

<style>
.action-btn {
    height: 20%;
    width: 80%;
    max-width: 600px;
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
</style>
