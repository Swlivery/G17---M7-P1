<template>
  <div class="container">
    <div class="logo-container">
      <img 
        src="https://imgur.com/haeXMBJ.png" 
        alt="¿Quién es ese Pokémon?" 
        class="game-logo"
      />
    </div>
    <h1>¿Quién es ese Pokémon?</h1>
    <div class="counter">
      Pokémon descubiertos: {{ discoveredCount }} / {{ totalPokemons }}
    </div>
    
    <div class="pokemon-grid">
      <PokemonCard
        v-for="pokemon in pokemons"
        :key="pokemon.id"
        :pokemon="pokemon"
        @correct-guess="handleCorrectGuess"
        @wrong-guess="showError"
      />
    </div>

    <ModalError
      :show="showErrorModal"
      @close="closeError"
    />
  </div>
</template>

<script>
import axios from 'axios'
import PokemonCard from './components/PokemonCard.vue'
import ModalError from './components/ModalError.vue'

export default {
  name: 'App',
  components: {
    PokemonCard,
    ModalError
  },
  data() {
    return {
      pokemons: [],
      showErrorModal: false
    }
  },
  computed: {
    discoveredCount() {
      return this.pokemons.filter(p => p.discovered).length
    },
    totalPokemons() {
      return this.pokemons.length
    }
  },
  methods: {
    async fetchPokemons() {
      try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20')
        const pokemonPromises = response.data.results.map(async (pokemon) => {
          const detailResponse = await axios.get(pokemon.url)
          return {
            ...detailResponse.data,
            discovered: false
          }
        })
        this.pokemons = await Promise.all(pokemonPromises)
      } catch (error) {
        console.error('Error fetching pokemons:', error)
      }
    },
    handleCorrectGuess(pokemonId) {
      const pokemon = this.pokemons.find(p => p.id === pokemonId)
      if (pokemon) {
        pokemon.discovered = true
      }
    },
    showError() {
      this.showErrorModal = true
    },
    closeError() {
      this.showErrorModal = false
    }
  },
  mounted() {
    this.fetchPokemons()
  }
}
</script>

<style>

.logo-container {
  text-align: center;
  margin-bottom: 2rem;
}
.game-logo {
  max-width: 500px;
  width: 100%;
  height: auto;
}
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
  margin-top: 2rem;
}

.counter {
  font-size: 1.5rem;
  text-align: center;
  margin: 1rem 0;
  color: #4CAF50;
  font-weight: bold;
}

h1 {
  text-align: center;
  color: #333;
}
</style>