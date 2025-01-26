<script>
import {computed, ref, watch} from 'vue'
import GameCard from './components/GameCard.vue' 
import WinnersPool from './components/WinnersPool.vue' 
import _ from 'lodash'
import {lounchSingle,lounchMultiple} from './firula/conffeti' 

export default {
  name: 'App',
  components: {
    GameCard,
    WinnersPool
  },
  setup(){
    //Cards
    const listCard = ref([])  
    const cardItems =[1,2,3,4,5,6,7,8,9,10]

    //Players
    const players = ['Lua','Artchi3','Floyd'];
    const playerList = ref([]);

    //UI
    const streaks = ref(0) 
    const rounds = ref(0)  
    
    // const playerName = ''
    // const inputPlayerScore = 0

    players.forEach((item)=> {
      var splitName = item.split("");
      playerList.value.push({
        name:item,
        winner: true,
        rounds: splitName.length
      })
    })

    //Make sort every time a player wins
    playerList.value = playerList.value.sort((a, b) => a.rounds - b.rounds)
    console.log(playerList.value)
    
    

    const shuffleCards = () =>{   
      listCard.value =  _.shuffle(listCard.value) 
      listCard.value = listCard.value.map((card,index)=>{
        return {...card,position:index}
      })
    }
    cardItems.forEach(item => {
      listCard.value.push({ 
        value:item,
        variant:1,
        visible:true,
        position:null,
        matched: false
      })
      listCard.value.push({
        value:item,
        variant:2,
        visible:true,
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
          streaks.value++ 
          setTimeout(() => {
            listCard.value[cardFirst.position].matched = true
            listCard.value[cardSecnd.position].matched = true 
            lounchSingle() 
          },600)
        }else{  
          
          rounds.value++
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
      restart,
      playerList,
      rounds,
      streaks 
    }
  }

}
</script>

<template> 
  <div class="game">
    <div class="missmatchUi"></div>
    <form action="" class="greetinggame">
      
      <h1>Bem vindo, insira seu nome de jogador para inciar</h1>
      <input v-model="playerName" type="text" placeholder="Artchi3">
      <button @click.prevent="restart">Iniciar</button>
      
    </form>
    <div class="pontuacao">
      <h2>Rodadas {{ rounds }}</h2>
      <h2>Match streaks {{ streaks }}</h2>
      
    </div>
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
      <div class="winners">
        <WinnersPool v-for="(players) in playerList"
        :key="`${players.name}`"
        :name="players.name"
        :winner="players.winner"
        :rounds="players.rounds"
        />
      </div>
      <button class="recomecar" @click="restart">
        Recomecar
      </button>
    </div>
  </div>
</template> 

<style>
html,body{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  background-color: #35654D;
}
.game{
  position: relative;
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff; 
  margin: 0;
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
.missmatchUi{
  display: none;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: rgba(194,2,2,0.38);
}
.comemoracao{
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: aqua;
  margin: 30px;
}
.winners{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  margin: 0px 30px 30px;
}
.pontuacao{
  width:55%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin: 30px auto;
}
.blink { 
  display: block;
} 
</style>

