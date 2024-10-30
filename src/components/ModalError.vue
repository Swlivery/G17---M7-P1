<template>
    <div v-if="show" class="modal-overlay">
      <div class="modal">
        <h3>¡Error!</h3>
        <p>El nombre ingresado es incorrecto. ¡Intenta de nuevo!</p>
        <button @click="$emit('close')">Cerrar</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ModalError',
    props: {
      show: {
        type: Boolean,
        required: true
      }
    }
  }
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
    z-index: 1000;
  }
  
  .modal {
    background: white;
    padding: 2rem;
    border-radius: 8px;
    text-align: center;
  }
  
  button {
    background: #4CAF50;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
    margin-top: 1rem;
  }
  </style>