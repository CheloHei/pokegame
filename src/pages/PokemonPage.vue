<template class="container">
  <h1 class="text-center" v-if="!pokemon">Espere por favor</h1>
  <div class="pokemon-page text-center" v-else>
    <h1 class="text-center">Quién es este Pokémon?</h1>
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <PokemonOptions :pokemons="pokemonArr" @selectPokemon="checkAnswer" />

    <template v-if="showAnswer" class="text-center">
      <h2 class="text-center">{{ message }}</h2>
      <button class="btn btn-primary text-center" @click="nextPokemon">Nuevo juego</button>
    </template>
  </div>
</template>

<script>
import PokemonPicture from "@/components/PokemonPicture.vue";
import PokemonOptions from "@/components/PokemonOptions.vue";

import getPokemonOptions from "@/helpers/getPokemonOptions";

export default {
  components: {
    PokemonPicture,
    PokemonOptions,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: "",
    };
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();
      const rndInt = Math.floor(Math.random() * 4);
      this.pokemon = this.pokemonArr[rndInt];
    },
    checkAnswer(pokemonId) {
      this.showAnswer = true;
      this.showPokemon = true;
      if (pokemonId === this.pokemon.id) {
        this.message = `Correcto ${this.pokemon.name}`;
      } else {
        this.message = `Incorrecto era ${this.pokemon.name}`;
      }
    },
    nextPokemon() {
      this.showAnswer = false
      this.showPokemon = false
      this.pokemon = null
      this.pokemonArr = []
      this.mixPokemonArray()
    },
  },
  mounted() {
    //once time
    this.mixPokemonArray();
  },
};
</script>
