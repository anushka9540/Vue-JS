<script setup>
import { ref } from 'vue';
import ModalProject from './components/ModalProject.vue';

const themes = ref('');
const inputField = ref(null);

function focusInputField() {
  if (inputField.value) {
    inputField.value.focus(); 
  }
}

function preventDirectFocus(event) {
  event.preventDefault(); 
}

</script>

<template>
  <div class="container">

    <div class="input-group">
      <button @click="focusInputField"><label>Enter Theme :</label></button>
      <input type="text" v-model="themes" placeholder="Enter theme (e.g. sales)" ref="inputField"  @mousedown="preventDirectFocus"/>
    </div>

    <h1>My First Vue App :)</h1>

    <ModalProject 
      modalTitle="Sign up for the Giveaway!"
      modalContent="Grab your ninja swag for half price!"
      :theme="themes"
    />
  </div>
</template>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 20px;
}


/* .input-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 15px;
  width: 100%;
} */

label {
  color: black;
  font-weight: 600;
  font-size: 1.1rem;
  margin-bottom: 5px;
  cursor: pointer;
}

input {
  width: 80%;
  max-width: 300px;
  padding: 8px;
  background: transparent;
  border: none;
  border-bottom: 2px solid;
  font-weight: 600;
  font-size: 1rem;
  margin-top: 20px;

}


body {
  background-color: rgb(105, 180, 183);
  margin: 0;
}
 
@media screen and (max-width: 600px) {
  h1 {
    font-size: 1.5rem;
  }
  input {
    font-size: 0.9rem;
    max-width: 250px;
  }
}
</style>
