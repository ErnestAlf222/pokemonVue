<template>
    <h2 v-if="!pokemon">Espere por favor...</h2>

    <div v-else>
        <h1>¿Quién es este Pokemón?</h1>
        <PokemoPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
        <PokemonOptions 
            :opciones="pokemonArr"
            @selection="checkAnswer"/>

            <template v-if="showAnswer">
                <h2 class="fade-in">{{ message }}</h2>
                <button class="again" @click="newGame">
                    Nuevo Juego
                </button>
            </template>
    </div>
  
</template>

<script>
import getPokemonOptions from '../helpers/getPokemonOptions'
import PokemonOptions from '@/components/PokemonOptions'
import PokemoPicture from '@/components/PokemoPicture'

// console.log(getPokemonOptions());

export default {
  components: { PokemonOptions,PokemoPicture },
  data(){
    return {
        pokemonArr:[],
        pokemon:null,
        showPokemon:false,
        showAnswer:false,
        message:''
    }
  },
  methods:{
    async mixPokemonArray(){
        this.pokemonArr = await getPokemonOptions()
        const rndInt = Math.floor(Math.random() * 4 )
        this.pokemon = this.pokemonArr[rndInt]
    },
    checkAnswer(selectedId){
        this.showPokemon=true
        this.showAnswer=true
        if (selectedId === this.pokemon.id) {
            this.message = `Correcto, ${this.pokemon.name }`;
            
        } else {
            this.message = `Upps, era ${this.pokemon.name}`
        }
    },
    newGame(){
        this.showPokemon = false;
        this.showAnswer= false;
        this.pokemonArr =[];
        this.pokemon=null;
        this.mixPokemonArray();

    }
},
mounted(){
    this.mixPokemonArray()
}


}
</script>
