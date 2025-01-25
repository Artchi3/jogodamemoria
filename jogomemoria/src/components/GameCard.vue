<script> 
//   import {emit} from 'vue'
  export default {
    name: 'GameCard', 
    props:{
        value:{
            type:Number,
            required:true
        },
        visible:{
            type:Boolean,
            default:true
        },
        position:{
            type:Number,
            required:true
        },
        matched:{
            type:Boolean,
            default:false
        }
    },
    setup(props, {emit}){
        const cardSelected =()=>{
            emit('card-select',{
                position:props.position,
                faceVal: props.value
            })
        }
        return{
            cardSelected
        }
    }
  }
  </script>

<template>  
    <div class="card" @click="cardSelected">
        <div v-if="visible" class="cardface front">
            {{ value }} -- {{ position }}
        </div>
        <div v-else class="cardface back">
            BACK{{ position }}
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
  .card{ 
    border: 2px solid black;
    position: relative;
  }
  .cardface{
    width: 100%;
    height: 100%;
    position: absolute;
  }
  .cardface.front{
    background-color: red;
    color:white
  }
  .cardface.back{
    background-color: blue;
    color:white
  }
  </style>
  