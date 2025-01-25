<script>
import {ref, watch} from 'vue'
import GameCard from './components/GameCard.vue' 

export default {
  name: 'App',
  components: {
    GameCard
  },
  setup(){
    const listCard = ref([])
    const userSelect = ref([])
    const status = ref([])

    for (let i = 0; i < 20; i++) {
      listCard.value.push({
        value:i,
        visible:false,
        position:i,
        matched: false
      }); 
    }

    const flipCard = (payload)=>{
      listCard.value[payload.position].visible = true
      if (userSelect.value[0]) {
        userSelect.value[1] = payload
      }else{
        userSelect.value[0] = payload
      }
    }

    watch(userSelect,(currValue)=>{ 
      if (currValue.length ===2) { 
        const cardFirst = currValue[0]
        const cardSecnd = currValue[1]

        if (cardFirst.faceVal === cardSecnd.faceVal) {
          status.value = 'Match'
          listCard.value[cardFirst.position].matched = true
          listCard.value[cardSecnd.position].matched = true 
        }else{
          status.value = 'Miss'
          listCard.value[cardFirst.position].visible = false
          listCard.value[cardSecnd.position].visible = false
        }
        
        userSelect.value.length = 0
      }
    },{deep:true})
    return{
      listCard,
      userSelect,
      flipCard,
      status
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
    <h2>{{ status }}</h2>
    <ul class="gamebord">
      <GameCard v-for="(card,index) in listCard"
        :key="`card-${index}`"
        :value="card.value"
        :visible="card.visible"
        :position="card.position"
        :matched="card.matched"
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
