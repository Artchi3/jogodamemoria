<script > 
export default{
  props:{
    value:{
      type:Number,
      required:true
    }
  }
}
</script>

<template>
  <div class="card"> 
     <div class="cardface is-front">{{ value }}
      </div>
      <div class="cardface is-back"></div>
  </div>
</template>

<style scoped>

</style>
