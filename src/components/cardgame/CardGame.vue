<script setup lang="ts">
import CardComponent from './CardComponent.vue';
</script>

<template> 
  <ul class="gameboard">
    <CardComponent/> 
  </ul>   
</template>

<style scoped>
.gameboard{
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px 100px;
  grid-column-gap: 50px ;
  grid-row-gap: 50px;
  justify-content: center;
  margin: 10px;
}
</style>
