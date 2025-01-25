<script>
import {computed, ref, watch} from 'vue'
import GameCard from './components/GameCard.vue' 
import _ from 'lodash'

export default {
  name: 'App',
  components: {
    GameCard
  },
  setup(){
    const listCard = ref([]) 

    const cardItems =[0,1,2,3,4,5,6,7,8,9]
    cardItems.forEach(item => {
      listCard.value.push({ 
        value:item,
        visible:false,
        position:null,
        matched: false
      })
      listCard.value.push({
        value:item,
        visible:false,
        position:null,
        matched: false
      })
    })
    listCard.value = listCard.value.map((card,index)=>{
      return {...card,position:index}
    })

    const userSelect = ref([])

    const status = computed(()=>{
      if(pairRamains.value === 0){
        return 'Vitoria!!'
      }else{
        return `Faltam ${pairRamains.value} Pares`
      }
    })

    const pairRamains = computed(()=>{
      const cardsRemaining = listCard.value.filter(card=>card.matched===false).length 
      return cardsRemaining / 2
    })
    
    

    const flipCard = (payload)=>{
      listCard.value[payload.position].visible = true
      if (userSelect.value[0]) {
        if((userSelect.value[0].position === payload.position) && (userSelect.value[0].faceVal === payload.faceVal)){return}
        userSelect.value[1] = payload
      }else{
        userSelect.value[0] = payload
      }
    }

    const shuffle = () =>{
      listCard.value = _.shuffle(listCard.value)
    }
    const restart =()=>{
      shuffle()
      listCard.value = listCard.value.map((card, index) => {
        return {
          ...card,
          matched:false,
          position:index,
          visible: false
        }
      })
    }
    watch(userSelect,(currValue)=>{ 
      if (currValue.length ===2) { 
        const cardFirst = currValue[0]
        const cardSecnd = currValue[1]

        if (cardFirst.faceVal === cardSecnd.faceVal) { 
          listCard.value[cardFirst.position].matched = true
          listCard.value[cardSecnd.position].matched = true 
        }else{ 
          setTimeout(() => {
            listCard.value[cardFirst.position].visible = false
            listCard.value[cardSecnd.position].visible = false
          }, 2000);
        }
        
        userSelect.value.length = 0
      }
    },{deep:true})
    return{
      listCard,
      userSelect,
      pairRamains,
      status,
      flipCard,
      shuffle,
      restart
      
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
      <button @click.prevent="shuffle"></button>
      
    </form>
    <h2>{{ pairRamains }}</h2>
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
      <h1 class="congratulation">{{ status }} Parabens!</h1>
      <table class="winners">
        
      </table>
      <button class="recomecar" @click="restart">
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
