<script setup lang="ts">
import type { Pokemon } from 'env';
import getPokemonOptions from './api/getPokemonOptions';
import { ref } from 'vue';
import PokemonOptions from './components/PokemonOptions.vue';  
import PokemonPicture from './components/PokemonPicture.vue';

  
  const pokemonArr = ref<Pokemon[]>([])
  const pokemon = ref<Pokemon>()
  const showPokemon = ref<boolean>(false)
  const showAnswer = ref<boolean>(false)
  const message = ref<string>()
  const streack = ref<number>(0)


  const mixPokemonArray =async()=>{
    pokemonArr.value = await getPokemonOptions()
    const ramdomInt = Math.floor(Math.random() *4)
    pokemon.value = pokemonArr.value[ramdomInt]
    console.log(pokemon.value)
    console.log(pokemonArr.value)
  }
  

  const checkAnswer =(selectedId: number)=>{
    showPokemon.value = true
    showAnswer.value = true

    if(selectedId === pokemon.value?.id){
      streack.value++
      message.value = `Correcto era un ${pokemon.value.name}`
    }
    else{
      streack.value = 0
      const motivationalMessage = [
      `Esta malo, es un ${pokemon.value?.name}`,
    ]  
    const randomInt = Math.floor(Math.random() *motivationalMessage.length)
    message.value = motivationalMessage[randomInt]
    }
  } 

const newGame =()=>{
  showPokemon.value = false
  showAnswer.value = false
  pokemonArr.value = []
  pokemon.value = undefined
  
  mixPokemonArray()  
}

mixPokemonArray()
</script>

<template>
  <div class="m-12">
    <PokemonPicture :show-pokemon="showPokemon" v-if="pokemon" :pokemon-id="pokemon.id" />
    <div class="flex items-center flex-col text-4xl font-bold mt-12">
      <p> {{ streack }} </p>
      <p v-if="streack == 10">Vas bien</p>

    </div>

    <PokemonOptions  :pokemon1="pokemon" :show-pokemon="showPokemon" :show-answer="showAnswer"  @selection-pokemon="checkAnswer" :pokemons="pokemonArr" />
  </div>
  <div v-if="showAnswer" class="flex flex-col text-center space-y-6">
      <p class="text-2xl font-bold">{{ message }}</p>
    <div>
      <button class="border-4 p-2 rounded-2xl" @click="newGame">Nuevo juego</button>
    </div>
  </div>

</template>
