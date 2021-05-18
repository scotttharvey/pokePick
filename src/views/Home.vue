<template>

  <!-- {{$route.params.slug}} -->
  <div class="w-full justify-items-center flex flex-col justify-items-center">
    <div class="m-auto">
      <input type="text" class="mb-20 border-blue-400 border-2 p-2 mt-10"
      v-model='text'
      placeholder="Search Pokemon"/>
    </div>

    <div class="text-2x text-blue-400">
      <div
      class=" pb10 auto-rows-min"
      v-for="(pokemon, idx) in filteredPokemon" :key="idx">
      <router-link :to="`/about/${urlIdLookup[pokemon?.name]}`">
        {{pokemon.name}}
      </router-link>
      </div>

    </div>

  </div>


</template>

<script>
// @ is an alias to /src

import { reactive, toRefs, computed } from 'vue'

export default {
  name: 'Home',
  setup(){

    const state = reactive({
      pokemons:[],
      urlIdLookup: {},
      text: '',
      filteredPokemon:computed(() => updatePokemon())
    })

    function updatePokemon() {
      if (!state.text) return []

      return state.pokemons.filter(pokemon =>
        pokemon.name.includes(state.text)
      )
    }

    const offset = 121

    fetch(`https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=39`)
    .then(res => res.json())
    .then(data => {
      state.pokemons = data.results;
      state.urlIdLookup = data.results.reduce((all, itm, idx) => {
        all = { ...all, [itm.name]:idx + 1 + offset}
        return all
      },{})
    })

    return {...toRefs(state)}
  }
}
</script>
