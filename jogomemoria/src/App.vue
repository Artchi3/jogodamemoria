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
    
    //Cards instance
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

    //Players Handlers
    const players = ['Lua','Artchi3','Floyd'];
    const playerList = ref([]);
    const playerScores =['3', '7', '5'];
    const playerName = ref('');

    //UI & controls
    const streaks = ref(0);
    const rounds = ref(0);
    const gameStarted = ref(false);
    const userSelect = ref([]);
    const missMatch =ref(false)

    //Computes args
    const isRound = computed(()=>{
      if(missMatch.value){
          return 'blink'
      }
      return ''
    })
    const isDisabled = computed(()=>{
          if(gameStarted.value){
            return true
          }
          return false
    });
    const winnerPrize = computed(()=>{
        if (pairRamains.value === 0) { 
          return 'is-active'
        }
        return ''
    }); 
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

    //Game essencial Functions
    const shuffleCards = () =>{   
      listCard.value =  _.shuffle(listCard.value) 
      listCard.value = listCard.value.map((card,index)=>{
        return {...card,position:index}
      })
    }
    const winnerHandle =() =>{ 
      playerList.value.splice(0); 
      playerScores.push(rounds)

      players.forEach((item,index)=> { 
          playerList.value.push({
            name:item,
            winner: true,
            rounds: playerScores[index]
          })
      })  
      
      playerList.value = playerList.value.sort((a, b) => a.rounds - b.rounds) 
    } 
    const flipCard = (payload)=>{
      listCard.value[payload.position].visible = true
      if (userSelect.value[0]) {
        if((userSelect.value[0].position === payload.position) && (userSelect.value[0].faceVal === payload.faceVal)){return}
        userSelect.value[1] = payload; 
      }else{
        userSelect.value[0] = payload
      }
    } 
    const startGame = () =>{
      if(playerName.value !== '' && playerName.value !== null && playerName.value.length > 2){ 
        
        if(players.indexOf(playerName.value) == -1){
          players.push(playerName.value);  
          gameStarted.value = true
          restart()
        } else{
          alert('Nome de jogador ja existe, escolha um novo')
        }
          
      }else{
        alert('Insira um nome de jogador com minimo de 3 letras antes de iniciar o jogo')
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
      rounds.value = 0
      streaks.value = 0
      shuffleCards();
    } 

    //Game Watcher
    watch(pairRamains, currValue =>{
      if(currValue === 0){
        winnerHandle()
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
          streaks.value = 0 
          setTimeout(() => {missMatch.value = true},300)
          setTimeout(() => {
            missMatch.value = false
            listCard.value[cardFirst.position].visible = false
            listCard.value[cardSecnd.position].visible = false
          }, 1500);
        }
        userSelect.value.length = 0
      }
    },{deep:true})
 
    return{
      listCard,
      userSelect, 
      playerList, 
      playerName, 
      playerScores,
      rounds,
      streaks,
      //Computed args
      pairRamains,
      winnerPrize,
      isDisabled,
      status,
      isRound,
      //functions
      flipCard,
      shuffleCards,
      startGame,
      restart
    }
  }

}
</script>

<template> 
  <div class="game">
    <div :class="isRound" class="missmatchUi"></div>
    <h1>Bem vindo, insira seu nome de jogador para inciar</h1>
    <form action="" class="greetinggame">
      <input minlength='3' maxLength='10' class="greentingInput" v-model="playerName" type="text" placeholder="Artchi3" :disabled='isDisabled' />
      <button class="startgame" @click.prevent="startGame">Iniciar</button>
    </form>
    <div class="pontuacao">
      <h2>Rodadas: <p>{{ rounds }}</p></h2>
      <h1>Player: <p>{{ playerName }}</p></h1>
      <h2>Match streaks: <p>{{ streaks }}</p></h2>
      
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
    <div :class="winnerPrize" class="comemoracao">
      <h1 class="congratulation">{{ status }}</h1>

      <div class="winners">
        <div class="headerP" >
          <p class="name">Player</p>
          <p class="score">Rodadas</p>
        </div>
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
  <footer><p>2025 - Built by Artchi3</p><p>Powered by Vue <img src="./assets/logo.png" alt="vue logo"></p></footer>
</template> 

<style>
html,body{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  background: url('../public/baralho/flat-pokertable.avif') center center repeat,#35654D;
  background-size: 12em;
  background-blend-mode: multiply;
}
footer{
  width: 100vw;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  background-color: #000; 
  position: sticky;
  bottom: 0;
  left: 0;
}
footer img{
  width: 15px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff; 
  margin: 0;
  width: 100vw;
  height: 100vh;
  overflow: scroll;
}
.game{
  position: relative;
  width: 100vw;
  height: auto;
  display: flex;
  flex-direction: column;
  margin: 50px 0;
}
h1,h2,h3,p,button,input{
  font-family: "Outfit", serif;
}
h1{
  margin:20px;
} 
.gamebord{
  display: grid;
  grid-template-columns: 75px 75px 75px 75px 75px ;
  grid-template-rows: 100px 100px 100px 100px  ;
  grid-row-gap: 25px ;
  grid-column-gap: 25px ;
  justify-content: center;
}
.greetinggame{
  display: flex;
  justify-content: center;
  align-items: center;  
}

.greentingInput{
  width: 200px;
  height: 44px;
  outline: none;
  border:2px solid #000;
  background-image:none;
  color: #000;
  background-color:#fff;
  -webkit-box-shadow: none;
  -moz-box-shadow: none;
  box-shadow: none;  
  padding-left:15px ;
  font-family: "Outfit", serif;
}
.greentingInput::placeholder{
  color:#000;
  opacity: 0.5;
  font-family: "Outfit", serif;
}
.shuffleAnimation{
  transition: transform 0.8s ease-in;
}
.comemoracao{
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: #5E2129;
  margin: 30px;
  visibility: hidden;
  opacity: 0;
  transition: visibility 0s, opacity 0.5s linear;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  width: 100vw;
  height: 100vh;
}
.comemoracao.is-active{
  visibility: visible;
  opacity: 1;
} 
button{
  width: 150px;
  height: 50px;
  color: #fff;
  background-color: #000;
  font-weight: 600;
  border: none;
  padding: 0; 
  cursor: pointer;
  outline: inherit;
  text-align: center; 
  display: flex;
  align-items: center;
  justify-content: center;
}
.recomecar::before{ 
  margin-right: 10px;
  display: block;
  width:25px;
  height: 25px;
  content: '';
  background: url('https://img.icons8.com/?size=25&id=11676&format=png&color=FFFFFF')
}
.startgame::before{ 
  margin-right: 10px;
  display: block;
  width:25px;
  height: 25px;
  content: '';
  background: url('https://img.icons8.com/?size=25&id=397&format=png&color=FFFFFF')
}
.winners{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  margin: 0px 30px 30px; 
  min-width: 250px;
  min-height: 250px;
}
.winners .headerP{
  display: grid; 
  grid-template-columns:  100px 100px ;
  grid-template-rows: auto;
  grid-row-gap: 0 ;
  grid-column-gap: 0 ; 
}
.winners .headerP p{
  border-bottom: 2px solid #000; 
  border-radius: 5px;
} 
.missmatchUi{
  display: none;
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto; 
  z-index: 2;
  background-color: rgba(194,2,2,0.38);
}
.pontuacao{
  width:55%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin: 30px auto;
} 
.pontuacao h1{
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 150px;
  height: 150px;
}
.blink{
  width: 100vw;
  height: 100vh;
  display: flex;
}
@media (max-width: 720px) {
  #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff; 
  margin: 0;
  width: 100vw;
  height: 100vh;
  overflow: scroll;
  }

  .game{
    position: relative;
    width: 100vw;
    height: auto;
    min-height: 1020px;
    display: flex;
    flex-direction: column;
    margin: 0 0 !important;
  }
  .gamebord{
    display: grid;
    grid-template-columns: 55px 55px 55px 55px !important;
    grid-template-rows: 75px 75px 75px 75px 75px !important;
    grid-row-gap: 25px ;
    grid-column-gap: 25px ;
    justify-content: center;
  }
  .greetinggame{
    display: flex;
    justify-content: center;
    align-items: center;  
  }

  .greentingInput{
    width: 200px;
    height: 44px;
    outline: none;
    border:2px solid #000;
    background-image:none;
    color: #000;
    background-color:#fff;
    -webkit-box-shadow: none;
    -moz-box-shadow: none;
    box-shadow: none;  
    padding-left:15px ;
    font-family: "Outfit", serif;
  }
  .greentingInput::placeholder{
    color:#000;
    opacity: 0.5;
    font-family: "Outfit", serif;
  }
  .shuffleAnimation{
    transition: transform 0.8s ease-in;
  }
  .comemoracao{
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    background-color: #5E2129;
    margin: 30px;
    visibility: hidden;
    opacity: 0;
    transition: visibility 0s, opacity 0.5s linear;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: 0 !important;
    width: 100vw;
    height: 100vh;
    min-height: 1020px;
  }
  .comemoracao.is-active{
    visibility: visible;
    opacity: 1;
  }   
  .pontuacao{
  width:100%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin: 30px auto;
} 
  .pontuacao h1{
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 150px;
    height: 150px;
  }
  .blink{
  width: 100vw; 
  display: flex;
  height: 1020px;
  }
}
</style>

