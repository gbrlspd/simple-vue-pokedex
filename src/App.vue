<template>
  <div id="App">
    <div class="column is-half is-offset-one-quarter">
      <input type="text" class="input is-rounded" placeholder="Search Pokémon..." v-model="search">
      <button id="search" class="button is-link is-rounded" @click="searchPokemon">Search!</button>
      <div v-for="(pokemon, index) in filteredPokemons" :key="pokemon.url">
      <PokemonInfo :num="index + 1" :name="pokemon.name.toUpperCase()" :url="pokemon.url" />
    </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonInfo from './components/PokemonInfo.vue';
export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      search: ''
    }
  },
  components: {
    PokemonInfo
  },
  created: function() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    }).catch(err => {
      console.log(err);
    })
  },
  methods: {
    searchPokemon: function() {
      this.filteredPokemons = this.pokemons;
      if(this.search == '' || this.search == ' ') {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.search);
      }
    }
  },
  computed: {
    /*
    searchResult: function() {
      if(this.search == '' || this.search == ' ') {
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.search);
      }
    }
    */
  }
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  #search {
    margin-top: 12px;
  }
</style>
