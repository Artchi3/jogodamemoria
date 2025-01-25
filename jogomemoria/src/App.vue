<script>
import {ref} from 'vue'
import GameCard from './components/GameCard.vue'

export default {
  name: 'App',
  components: {
    GameCard
  },
  setup(){
    const listCard = ref([])
    
    for (let i = 0; i < 20; i++) {
      listCard.value.push({
        value:i,
        visible:false,
        position:i
      }); 
    }

    const flipCard = (payload)=>{
      listCard.value[payload.position].visible = true
    }
    return{
      listCard,
      flipCard
    }
  }
}
</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png">
   
  <div class="game">
    <form action="" class="greetinggame">
      <h1>Bem vindo, insira seu nome de jogador para inciar</h1>
      <input type="text">
      <button></button>
    </form>
    <ul class="gamebord">
      <GameCard v-for="(card,index) in listCard"
        :key="`card-${index}`"
        :value="card.value"
        :visible="card.visible"
        :position="card.position"
        @card-select="flipCard"
      /> 
    </ul>
    <div class="comemoracao">
      <h1 class="congratulation">Parabens!</h1>
      <table class="winners">

      </table>
      <button class="recomecar">
        recomecar
      </button>
    </div>
  </div>
</template> 

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.gamebord{
  display: grid;
  grid-template-columns: 100px 100px 100px 100px ;
  grid-template-rows: 100px 100px 100px 100px 100px ;
  grid-row-gap: 30px ;
  grid-column-gap: 30px ;
  justify-content: center;
}
</style>
