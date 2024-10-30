<template>
    <div class="pokemon-card">
      <img 
        :src="pokemon.sprites.front_default" 
        :class="{ 'hidden-pokemon': !pokemon.discovered }"
        :alt="pokemon.name"
      />
      <div v-if="!pokemon.discovered">
        <input 
          v-model="guessName"
          @keyup.enter="checkGuess"
          type="text" 
          :placeholder="'¿Quién es este Pokémon?'"
        />
        <button @click="checkGuess">Descubrir</button>
      </div>
      <h3 v-else>{{ pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1) }}</h3>
    </div>
  </template>
  
  <script>
  export default {
    name: 'PokemonCard',
    props: {
      pokemon: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        guessName: ''
      }
    },
    methods: {
      checkGuess() {
        if (this.guessName.toLowerCase() === this.pokemon.name.toLowerCase()) {
          this.$emit('correct-guess', this.pokemon.id)
        } else {
          this.$emit('wrong-guess')
        }
        this.guessName = ''
      }
    }
  }
  </script>
  
  <style scoped>
  .pokemon-card {
    border: 1px solid #ccc;
    border-radius: 8px;
    padding: 1rem;
    text-align: center;
    background: #f5f5f5;
    margin: 1rem;
    width: 200px;
  }
  
  .hidden-pokemon {
    filter: blur(5px) grayscale(100%);
  }
  
  img {
    width: 150px;
    height: 150px;
  }
  
  input {
    margin: 0.5rem 0;
    padding: 0.5rem;
    width: 80%;
  }
  
  button {
    background: #4CAF50;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
  }
  
  button:hover {
    background: #45a049;
  }
  </style>