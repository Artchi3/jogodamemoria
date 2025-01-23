<script setup>
import GameCard from './GameCard.vue';
defineProps({
  msg: {
    type: String,
    required: true,
  },
})

</script>

<template>
  <div class="game">
    <h3>Insira seu nome de jogador para começar!</h3>
    <form action="">
        <input type="text" name="" id=""/>
        <button>INICIAR!</button>
    </form>
    <ul class="gameboard">
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
        <li class="card"></li>
    </ul>
    <div>

    </div>
  </div>
</template>

<style scoped>
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
