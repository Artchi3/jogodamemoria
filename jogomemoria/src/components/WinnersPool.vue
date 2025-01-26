<script>
// import { computed } from 'vue'; 
  export default {
    name: 'WinnersPool', 
    props:{
        name:{
            type:String,
            required:true
        },
        winner:{
            type:Boolean,
            default:true
        },
        rounds:{
            type:Number,
            required:true
        } 
    },
    setup(props, {emit}){
        const playerEntry =()=>{
            emit('player-selection',{
                name:props.name,
                winner: props.winner,
                rounds: props.rounds
            })
        }
        return{ 
            playerEntry
        }
    }
  }
  </script>

<template>  
    <div class="player" @click="playerEntry">
        <p class="name">{{ name }}</p>
        <p class="score">{{ rounds }}</p>
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
  
  .player{
    display: grid; 
    grid-template-columns:  100px 100px ;
    grid-template-rows: auto;
    grid-row-gap: 0 ;
    grid-column-gap: 0 ; 
    justify-content: center; 
  } 
  .player p{
    border: 2px solid #000; 
    border-top: none;
    border-radius: 1px;
    }
  </style>
  