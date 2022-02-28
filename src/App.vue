<template>
  <pokemon-cards 
    :pokemons="pokemons"
    :selectedID="selectedID"
    @pokemonClicked="fetchEvolutions" 
  />

  <pokemon-cards :pokemons="evolutions" />
</template>

<script>
import PokemonCards from './components/PokemonCards.vue';
const api = 'https://pokeapi.co/api/v2/pokemon';
const STARTER_IDS = [1, 4, 7]

export default {
  data() {
    return {
      pokemons: [],
      evolutions: [],
      selectedID: null
    }
  },
  components: {
    PokemonCards
  },
  async created() {
    const starters = await this.fetchData(STARTER_IDS);
    this.pokemons = starters;
  },
  methods: {
    async fetchEvolutions(pokemon) {
      this.selectedID = pokemon.id
      this.evolutions = await this.fetchData([pokemon.id + 1, pokemon.id + 2])
    },
    async fetchData(ids) {
      // Promise.all will await for several asynchronous task (wait for these three calls to finish)
      const responses = await Promise.all(ids.map(id => fetch(`${api}/${id}`) ));
      const data = await Promise.all(responses.map(res => res.json()));
      return data.map(datum => ({ // minimal information from api
        id: datum.id,
        name: datum.name,
        sprite: datum.sprites.other['official-artwork'].front_default,
        types: datum.types.map(tp => tp.type.name)  
      }))
    },
  }
}
</script>