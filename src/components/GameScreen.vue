<script setup lang="ts">
import CardGame from './cardgame/CardGame.vue';
defineProps<{
  msg: string
}>()
</script>

<template>
  <div class="greetings">
    <!-- Apresentação -->
    <h1 class="green">Bem vindo ao jogo da memoria, insira seu nome para começar!</h1>
    <!-- REQUISIÇÃO DE NOME -->
    <form action=""> 
        <input v-model="text" @input="onInput" required placeholder="Insira seu nome de jogador">
        <p></p>
        <button></button>
    </form>
    <!-- JOGO ATIVO - LISTA DE CARTAS -->
    <h3>CONTADOR DE RODADAS: {{ RODADAS }}</h3> 
    <CardGame></CardGame>
    <!-- ALERTA DE COMEMORAÇÃO -->
    <div>
        <h3></h3>
        <!-- Ranking de jogadores que demporaram menos rodadas -->
        <ul>
            <li>

            </li>
        </ul>
        <button></button>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
