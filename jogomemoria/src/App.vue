<script>
import {computed, ref, watch} from 'vue'
import GameCard from './components/GameCard.vue' 
import _ from 'lodash'
import {lounchSingle,lounchMultiple} from './firula/conffeti'

export default {
  name: 'App',
  components: {
    GameCard
  },
  setup(){
    const listCard = ref([]) 

    const cardItems =[1,2,3,4,5,6,7,8,9,10]

    const shuffleCards = () =>{   
      listCard.value =  _.shuffle(listCard.value)
      console.log(listCard)
      console.log(listCard.value) 
      listCard.value = listCard.value.map((card,index)=>{
        return {...card,position:index}
      })
    }
    cardItems.forEach(item => {
      listCard.value.push({ 
        value:item,
        variant:1,
        visible:false,
        position:null,
        matched: false
      })
      listCard.value.push({
        value:item,
        variant:2,
        visible:false,
        position:null,
        matched: false
      })
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
        userSelect.value[1] = payload; 
      }else{
        userSelect.value[0] = payload
      }
    }

    
    const restart =()=>{
      
      listCard.value = listCard.value.map((card, index) => {
        return {
          ...card,
          matched:false,
          position:index,
          visible: false
        }
      });
      shuffleCards();
    }

    watch(pairRamains, currValue =>{
      if(currValue === 0){
        lounchMultiple()
      }
    })
    watch(userSelect,(currValue)=>{ 
      if (currValue.length ===2) { 
        const cardFirst = currValue[0]
        const cardSecnd = currValue[1]

        if (cardFirst.faceVal === cardSecnd.faceVal) { 
          setTimeout(() => {
            listCard.value[cardFirst.position].matched = true
            listCard.value[cardSecnd.position].matched = true 
            lounchSingle()
          },600)
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
      shuffleCards,
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
      <input type="text" placeholder="Artchi3">
      <button @click.prevent="shuffleCards">Iniciar</button>
      
    </form>
    <h2>{{ pairRamains }}</h2>
    <transition-group tag="shuffleAnimation" class="gamebord">
      <GameCard v-for="(card) in listCard"
        :key="`${card.value}-${card.variant}`"
        :value="card.value"
        :visible="card.visible"
        :position="card.position"
        :matched="card.matched"
        @card-select="flipCard"
      /> 
    </transition-group>
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
body{
  background-color: #35654D;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  margin-top: 60px;
  
}
.gamebord{
  display: grid;
  grid-template-columns: 75px 75px 75px 75px 75px ;
  grid-template-rows: 100px 100px 100px 100px  ;
  grid-row-gap: 25px ;
  grid-column-gap: 25px ;
  justify-content: center;
}
.shuffleAnimation{
  transition: transform 0.8s ease-in;
}
</style>
