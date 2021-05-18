<template>
  <div v-if="pokemon"
  class="w-7/12 m-auto bg-purple-100 mt-4 m-20 shadow-2xl flex justify-center flex-col items-center">
    <router-link :to="`/`" class="mr-auto">
    <div class="ml-12 mt-8 p-5 m-auto bg-white shadow-2xl w-48 h-30 text-center rounded">
      Back
    </div>
    </router-link>

    <h3 class="text-2xl m-auto mt-10 uppercase">{{pokemon.name}}</h3>
    <div class="flex flex-row flex-1">
      <img class="w-48" :src="pokemon.sprites.front_shiny" />
      <img class="w-48" :src="pokemon.sprites.back_shiny" />
    </div>
    <h3 class="text-yellow-500 mb-10">Types</h3>
    <div class="flex flex-wrap space-x-2 space-y-2 h-full h-auto pb-8">
      <div v-for="(type, idx) in pokemon.types" :key="idx"
      class="">
        <h5 class="text-red-600 uppercase p-4 bg-white mb-auto">{{type.type.name}}</h5>
      </div>
    </div>

  </div>
</template>

<script>
import { useRoute } from "vue-router"
import { reactive, toRefs } from 'vue';

export default {

  setup () {
    const route = useRoute();

    const state = reactive({
      pokemon: null,
    })

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
    .then(res => res.json())
    .then(data => {
      console.log(data)
      state.pokemon = data
      return data
    })

    return { ...toRefs(state) }
  }
}
</script>

<style lang="scss" scoped>

</style>