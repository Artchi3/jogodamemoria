<script setup> 

defineProps({
  msg: {
    type: String,
    required: true,
  },
})

</script>

<template>
  
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
