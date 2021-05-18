<template>

  <!-- {{$route.params.slug}} -->
  <div class="w-full justify-items-center flex flex-col justify-items-center">
    <div class="m-auto">
      <input type="text" class="mb-5 border-blue-400 border-2 p-2 mt-3"
      v-model='text'
      placeholder="Search Pokemon"/>
    </div>

    <div class="text-base text-blue-400 flex justify-center max-w-full m-auto flex-wrap space-x-2">
      <div
      class="m-2 w-42 sm:w-16 md:w-32 sm:w-full bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center"
      v-for="(pokemon, idx) in filteredPokemon" :key="idx">
      <router-link :to="`/about/${urlIdLookup[pokemon?.name]}`" class="p-3 text-center font-bold sm:text-base text-base w-full">
        {{pokemon.name.toUpperCase()}}
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
