<template>
  <div class="app-wrapper" >
    <div class="app-container" :class="{ 'blur-background': showModal }">
      <div class="input-group">
        <button v-on:click="focusInputField"><label>Focus</label></button>
        <input 
          type="text" 
          v-model="theme" 
          placeholder="Enter theme (e.g. sales)" 
          ref="inputField"  
        />
      </div>

      <h1>My First Vue App :)</h1>

      <button v-on:click="toggleModal" class="open-modal">Open Modal</button>
    </div>

    <ModalProject 
      v-if="showModal"
      :modalTitle="'Sign up for the Giveaway!'"
      :modalContent="'Grab your ninja swag for half price!'"
      :theme="theme"
      v-on:close="toggleModal"
    >

      <p>Enjoy exclusive deals and offers by signing up today!</p>


      <template v-slot:links>
        <a href="#" class="modal-link">Learn More</a>
        <a href="#" class="modal-link">Sign Up</a>
      </template>
    </ModalProject>
    
  </div>
</template>

<script>
import ModalProject from './components/ModalProject.vue';

export default {
  name:'App',
  components: {
    ModalProject
  },
  data() {
    return {
      theme: '',
      inputField: null,
      showModal: false
    };
  },
  methods: {
    focusInputField() {
      if (this.$refs.inputField) {
        this.$refs.inputField.focus();
      }
    },
    toggleModal() {
      this.showModal = !this.showModal;
    }
  }
};
</script>

<style>
.app-wrapper {
  position: relative;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 20px;
  width: 100%;
  height: 100%;
  transition: filter 0.3s ease-in-out;
}

.blur-background {
  filter: blur(5px);
}

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

h1 {
  color: #000000cf;
}

body {
  background-color: rgb(213, 217, 217);
  margin: 0;
}

.open-modal {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.modal-link {
  display: inline-block;
  margin: 10px;
  padding: 8px 15px;
  text-decoration: none;
  color: white;
  background-color: #007bff;
  border-radius: 5px;
  font-weight: bold;
  transition: 0.3s ease;
}

.modal-link:hover {
  background-color: #0056b3;
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