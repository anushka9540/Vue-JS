<template>
    <div>
        <span>
            {{ gameStarted ? "Pay attention. Click stop when the color changes." : "Click Go to test your reaction time!" }}
        </span>
        <br>
        <span v-if="tooSoonMessage">{{ tooSoonMessage }}</span>
    
        <span v-else-if="reactionTime && !modalVisible" class="span-result">
            Your reaction time was {{ reactionTime }}.
        </span>
    
        <span v-if="highScore" class="high-score">High Score: {{ highScore }}</span>
    </div>
</template>

<script>
export default {
  name: 'GameResult',
  props: ['gameStarted', 'reactionTime', 'highScore', 'tooSoonMessage', 'modalVisible']
};
</script>

<style>
.result {
    height: 25%;
    width: 80%;
    max-width: 600px;
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
</style>
