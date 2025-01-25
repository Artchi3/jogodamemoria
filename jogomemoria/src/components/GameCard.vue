<script>
import { computed } from 'vue'; 
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
        const animatedStyles = computed(()=>{
            if (props.visible) {
                return 'is-flipped'
            }
            return ''
        })
        const cardSelected =()=>{
            emit('card-select',{
                position:props.position,
                faceVal: props.value
            })
        }
        return{
            cardSelected,
            animatedStyles
        }
    }
  }
  </script>

<template>  
    <div class="card" :class="animatedStyles" @click="cardSelected">
        <div  class="cardface front">
            <label class="checkmark" v-if="matched"></label>
            <img :src="`/baralho/${value}.PNG`" :alt="value">
            {{ value }} {{ position }} -- {{ faceVal }} 
            
        </div>
        <div class="cardface back"> {{ value }} {{ position }} -- {{ faceVal }} 
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
  .checkmark{
    display: block;
    width: 35px;
    height: 35px;
    position: absolute;
    bottom: 35%;
    right: 25%;
    border-radius: 100%;
    background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64" enable-background="new 0 0 64 64"><path d="M32,2C15.431,2,2,15.432,2,32c0,16.568,13.432,30,30,30c16.568,0,30-13.432,30-30C62,15.432,48.568,2,32,2z M25.025,50 l-0.02-0.02L24.988,50L11,35.6l7.029-7.164l6.977,7.184l21-21.619L53,21.199L25.025,50z" fill="%2343a047"/></svg>'), #ffffff;
}
  .card{ 
    transition: 0.5s transform ease-in;
    transform-style: preserve-3d;
    position: relative;
  }
  .card.is-flipped{
    transform: rotateY(180deg);
  }
  .cardface{
    width: 100%;
    height: 100%;
    position: absolute;
    border-radius: 10px;
    border: 2px solid black;
    display: flex;
    flex-direction: column;
    align-items: center;
    backface-visibility: hidden;
  }
  .cardface img{
    width: 100%;
    height: 100%;
    display: flex; }
  .cardface.front{
    background-color: red;
    color:white;
    transform: rotateY(180deg);
  }
  .cardface.back{
    background-image: url('../assets/baralho/back.PNG');
    background-position: center;
    color:white;
    
  }
  </style>
  