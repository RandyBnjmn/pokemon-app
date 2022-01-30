<template>
  <h1 v-if="!pokemon">Por favor espere...</h1>
  <div v-else>
    <h1>Quien es este pokemon?</h1>

    <!-- TODO: Img -->
    <PokemonImage :pokemonId="pokemon.id" :showPokemon="showPokemon" />

    <!-- TODO: Opciones -->
    <PokemonOptions :pokemons="pokemonArr" @selection-pokemon="checkAsnwer" />

    <div v-if="showAnswer">
      <h2>{{ message }}</h2>
      <button @click="newGame">Nuevo juego</button>
    </div>
  </div>
</template>

<script>
  import PokemonImage from '../components/PokemonImage.vue';
  import PokemonOptions from '../components/PokemonOptions.vue';
  import getPokemonOptions from '@/helpers/getPokemonOptions';

  console.log(getPokemonOptions());
  export default {
    name: 'Pokemon Page',
    data() {
      return {
        pokemonArr: [],
        pokemon: null,
        showPokemon: false,
        message: '',
        showAnswer: false,
      };
    },
    components: {
      PokemonImage,
      PokemonOptions,
    },
    methods: {
      async mixPokemonArray() {
        this.pokemonArr = await getPokemonOptions();
        const rndInt = Math.floor(Math.random() * 4);

        this.pokemon = this.pokemonArr[rndInt];
      },
      newGame() {
        this.showPokemon = false;
        this.showAnswer = false;
        this.pokemonArr = [];
        this.pokemon = null;
        this.mixPokemonArray();
      },

      checkAsnwer(pokemonId) {
        this.showPokemon = true;
        this.showAnswer = true;
        if (pokemonId == this.pokemon.id) {
          this.message = `Correcto, ${this.pokemon.name}`;
        } else {
          this.message = `Oops, era ${this.pokemon.name} `;
        }
      },
    },
    mounted() {
      this.mixPokemonArray();
    },
  };
</script>
