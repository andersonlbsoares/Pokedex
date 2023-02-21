<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <hr> 
      <h2 class="title is-1 is-spaced">Pokedex</h2>
      <hr>
      <input class="input is-primary is-rounded" type="text" v-model="busca" placeholder="Search Pokemon">
      <button class="button is-primary is-rounded mt-3" @click="search" >Search</button>
      <button class="button is-primary is-rounded mt-3 ml-3" @click="redefine" >Redefine</button>
      <div v-for="(poke) in filteredPokemons" :key="poke.url" >
        <PokemonOnly :name="poke.name" :url="poke.url"/>
      </div>
    </div>

  </div>
</template>

<script>


import axios from 'axios';
import PokemonOnly from './components/PokemonOnly.vue';

export default {
  name: 'App',
  
  components: {
    PokemonOnly
  },
 
  
  data(){
    return{
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    } 
  },

  created: function() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0')
      .then(response => {
        this.pokemons = response.data.results;
        this.filteredPokemons = response.data.results;
        console.log(this.pokemons);
      })
      .catch(function(error) {
        console.log(error);
      });
  },
  methods:{
      search: function(){
        var seach = this.busca;
        var search = seach.toLowerCase();
        this.filteredPokemons = this.pokemons;
        if (search == '' || search == ' '){
          this.filteredPokemons = this.pokemons;
        }else{
          this.filteredPokemons = this.pokemons.filter(pokemon =>pokemon.name.includes(search));
        }
      },

      redefine: function(){
        this.filteredPokemons = this.pokemons;
      }
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
</style>
