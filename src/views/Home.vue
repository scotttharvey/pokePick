<template>

  <!-- {{$route.params.slug}} -->
  <div class="w-full justify-items-center flex flex-col justify-items-center">
    <div class="m-auto">
      <input type="text"
      id="pokemonInput"
      class="mb-5 border-blue-400 border-2 p-2 mt-3 rounded"
      v-model='searchText'
      placeholder="Search Pokemon"/>
    </div>

    <teleport to="#notApp" :disabled="!showApp" >
      <img src="../assets/logo.png" alt="" class="w-32 m-auto">
    </teleport>

    <button @click="showApp = !showApp" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded w-2/5 m-auto">Teleport Content</button>

    <div class="text-base text-blue-400 flex justify-center max-w-full m-auto flex-wrap space-x-2">
      <div
      class="m-2 w-42 sm:w-16 md:w-32 sm:w-full bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center rounded "
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
      searchText: '',
      filteredPokemon:computed(() => updatePokemon())
    })

    function updatePokemon() {
      // if (!state?.searchText && !state?.pokemons) return []

      // state?.pokemons?.filter(pokemon =>{
      //   // console.log(pokemon.name.includes(state.searchText));
      //   let txt = `${state.searchText}`
      //   console.log(pokemon.name.match(txt));

      //   }
      // )

      return state?.pokemons?.filter(pokemon => {
        return pokemon.name.match(state.searchText)
        }
      )
    }

    const offset = 0
    // const apiCondition = state?.pokemons

    fetch(`https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=280`)
    .then(res => res.json())
    .then(data => {
      console.log('NO');
      console.log(data.results);
      state.pokemons = data.results;
      state.urlIdLookup = data.results.reduce((all, itm, idx) => {
        all = { ...all, [itm.name]:idx + 1 + offset}
        return all
      },{})
    })

    return {...toRefs(state)}
  },
  data() {
    return {
      showApp: false
    }
  }
}
</script>
