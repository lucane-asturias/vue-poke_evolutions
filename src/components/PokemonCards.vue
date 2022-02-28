<template>
  <div class="cards">
    <card 
      v-for="pokemon in pokemons" 
      @click="handleClick(pokemon)"
      :class="{ opace: selectedID && pokemon.id !== selectedID }"
      class="card"
    >
      <template v-slot:title>
        {{ pokemon.name }} #{{ pokemon.id }}
      </template>
      <template v-slot:content>
        <img :src="pokemon.sprite">
      </template>
      <template v-slot:description>
        <div v-for="type in pokemon.types">
          {{ type }}
        </div>
      </template>
    </card>
  </div>
</template>

<script>
import Card from './Card.vue';

export default {
  name: 'PokemonCards',
  data () {
    return {}
  },
  components: {
    Card,
  },
  props: {
    pokemons: {
      type: Array
    },
    selectedID: {
      type: Number
    }
  },
  methods: {
    handleClick(pokemon) {
      this.$emit('pokemonClicked', pokemon)
    }
  }
}
</script>

<style scoped>
.cards {
  display: flex;
}

.opace {
  opacity: 0.5;
}

.card:hover {
  opacity: 1.0;
}

img {
  width: 100%;
}
</style>