<template>
    <div class="modal-overlay" v-on:click="closeModal">
      <div class="modal-box" :class="themeClass">
        <div class="modal-header">
          <h2>{{ modalTitle }}</h2>
        </div>
        <div class="modal-body">
          <p>{{ modalContent }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name:'ModalProject',
    props: {
      modalTitle: String,
      modalContent: String,
      theme: String
    },
    computed: {
      themeClass() {
        return this.theme === 'sales' ? 'sales-theme' : 'default-theme';
      }
    },
    methods: {
      closeModal() {
        this.$emit('close');
      }
    }
  };
  </script>

  <style scoped>
  .modal-overlay {
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
  
  .modal-box {
    height: 200px;
    width: 90%;
    max-width: 400px;
    border-radius: 20px;
    box-shadow: 8px 8px 10px rgba(0, 0, 0, 0.6);
    padding: 20px;
    text-align: center;
    transition: all 0.3s ease-in-out;
    background: white;
    z-index: 10;
  }
  
  .default-theme {
    background-color: #ffffff;
    color: rgb(21, 131, 139);
  }
  
  .sales-theme {
    border: none;
    background-color: rgb(203, 33, 33);
    color: #ffffff;
  }
  
  .modal-body {
    margin-top: 20px;
    font-size: 1rem;
  }
  
  @media screen and (max-width: 600px) {
    .modal-box {
      width: 95%;
      height: auto;
      padding: 15px;
    }
    h2 {
      font-size: 1.2rem;
    }
    p {
      font-size: 0.9rem;
      margin-top: 15px;
    }
  }
  </style>