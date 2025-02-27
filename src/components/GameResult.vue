<template>
    <div>
        <span>
            {{ gameStarted ? "Pay attention. Click stop when the color changes." : "Click Go to test your reaction time!" }}
        </span>
        <br>
        <span v-if="result">
            {{ result }}
        </span>
        <span v-if="highScore" class="high-score">High Score: {{ highScore }}s</span>
    </div>
</template>

<script>
export default {
  name: 'GameResult',
  props: ['gameStarted', 'highScore', 'result']
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