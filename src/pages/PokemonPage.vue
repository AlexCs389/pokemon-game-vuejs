<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>
  <div v-else>
    <h1>¿Quién es este pokémon</h1>
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer" />

    <template v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame()">Nuevo juego</button>
    </template>
  </div>
</template>

<script>
import PokemonPicture from '@/components/PokemonPicture';
import PokemonOptions from '@/components/PokemonOptions';

import getPokemonOptions from '@/helpers/getPokemonOptions';

export default {
  components: {
    PokemonPicture,
    PokemonOptions
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: ''
    }
  },
  methods: {
    async mixPokemonArr () {
      this.pokemonArr = await getPokemonOptions();
      this.pokemon = this.pokemonArr[Math.floor(Math.random() * 4)];
    },
    checkAnswer(event) {
      this.showPokemon = true;
      this.showAnswer = true;
      this.message = (this.pokemon.id === event) ? `Correcto, ${ this.pokemon.name }` : `Oops, era ${ this.pokemon.name }`;
    },
    newGame() {
      this.pokemonArr = [];
      this.pokemon = null;
      this.showPokemon = false;
      this.showAnswer = false;
      this.mixPokemonArr();
    }
  },
  mounted() {
    this.mixPokemonArr();
  }
}
</script>

<style>

</style>