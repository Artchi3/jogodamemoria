<script setup> 
import GameCard from './components/gamecomponents/GameCard.vue';

defineProps({
  msg: {
    type: String,
    required: true,
  },
})
setup(){
    const cardList =[]
    for (let i = 0; i < 20; i++) {
        cardList.push(i)
        
    }
}
</script>

<template> 
  <main> 
    <div class="game">
      <h3>Insira seu nome de jogador para começar!</h3>
      <form action="">
          <input type="text" name="" id=""/>
          <button>INICIAR!</button>
      </form>
      <ul class="gameboard">
          <GameCard  v-for="(card, index) in cardList" class="card" :key="`card-${index}`" :value="card"/>
          <li></li>
      </ul>
    <div>

    </div>
  </div>
  </main>
</template>

<style scoped>
main{
  display: flex;
  align-items: center;
  justify-content: center;
}
.game{
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;

}
.gameboard{
    display: grid;
    grid-template-columns: 100px 100px 100px 100px ;
    grid-template-rows: 100px 100px 100px 100px 100px;
    grid-column-gap: 30px;
    grid-row-gap: 30px;
    margin: 10px;
    justify-content: center;
}
.card{
    border: 2px solid red;
    list-style: none;
}
</style>
