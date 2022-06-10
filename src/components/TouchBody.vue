<template>
  <audio ref="audio" autoplay="autoplay" v-if="last >= 1">
    <source src="../audio/play1.wav" type="audio/mpeg">
  </audio>
  <div>
    <div id="checking"><h1>{{value}}</h1>{{checking}}</div>
    <div style="width:100%;">      
       <img :class="{'polegar-green' : press != null}" 
      id="polegar" class="polegar" src="../assets/button.png" 
      @mousedown="pulse" @keydown="pulse" @touch:start="pulse" @mouseup="stop" @keyup="stop" @keypress="setPressed"  @click="setPressed">  
    </div>    
    <!-- <button @mousedown="pulse" @touch:start="pulse" @mouseup="stop" @click="setPressed" > :::Press Here::: </button>     -->
  </div>   
    <div class="check">
      <div class="hits">  
        {{hits}} |       
        Hits 
        <!-- <img src="../assets/ok.png" style="height: 15px" alt="">          -->
      </div>
      <div class="time">{{jogada}}</div>      
      <div class="errors">        
        <!-- <img src="../assets/nok.png" style="height: 15px" alt="">   -->
        Erros | 
        {{errors}}     
      </div>
    </div>  
</template>

<script>
import Play from '../views/Play.vue'
export default {
  name: "TouchBody",   
  data(){ 
    return{
      value: 0,
      press: null,      
      pressed: 0,
      jogada: [],
      hits: 0,
      errors:0,
      bpm: 60,
      interval: 1000,      
    } 
  },
  props: {
    shorted: Object,  
    last: Number,  
    play: Boolean,      
  },  
  computed:{   
    checking(){      
      this.checkHitsErrors();
    },
  },
  methods:{
    pulse(){   
      if(this.value === 0){this.value = 1;}   
      //O TIMER ESTÁ ZERADO OU PARADO 
      this.press = setInterval(() =>{        
        this.value++;         
      }, this.interval); //1 SEGUNDO = 1000 MILESEGUNDOS         
               
     },
    stop(){
      clearInterval(this.press);           
      this.jogada.push(this.value);
      this.press = null;   
      this.value = 0;

      if(this.last > 3) this.checkHitsErrors();
    },
    checkHitsErrors(){  
      console.log("Pressed " + this.pressed, "Jogada: " + this.jogada);  
      if((this.jogada.length > 0 && this.last === 4)){
        if(this.value === 0 && this.pressed === 4 && this.press === null){                         
          if(this.shorted.toString() === this.jogada.toString()){
            this.$emit('new-short') 
            this.plusHits();  
            this.clear();   
          }else{
            this.plusErrors();
            this.clear();               
          }         
        }       
      }

      if(this.value > 4){
        this.plusErrors();
        this.clear();  
      }

      if(this.jogada.length > 0 && this.last === 1 && this.pressed > 1){
        this.plusErrors();
        this.clear();
      }
    },
    clear(){
      this.jogada.splice(0, this.jogada.length);  
      this.pressed = 0; 
      this.value = 0;
    },  
    plusHits(){
      this.hits++;
      this.clear();
         
    },
    plusErrors(){
      this.errors++;
      this.clear();
    },  
    setPressed(){
      this.pressed = this.last;
      console.log("Pressed "+ this.pressed);
    }, 
    monted(){
        document.getElementById('polegar').focus();   
    },   
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
   button{
    width: 50%;  
    height: 50px;
    background-color: blue;   
    border-radius: 10px; 
    color: white;
  }

  .check{
    border: solid black;
    display: flex;
    flex-direction: row;     /* make main axis horizontal (default setting) */
    justify-content: center; /* center items horizontally, in this case */
    align-items: center; 
  }
  .hits, .errors{
    width: 25%;
    background-color: white;
    font-weight: bold;
    font-size: 20px;
  } 
  .time{
    width: 50%;
    background-color: white;
    font-weight: bold;
    font-size: 20px;
  }

  .select-none{
    user-select: none!important;
  }

  .ban{
    display: inline-flex;
  }

  .ok{
    display: flex;
    padding: 0px;    
    margin: 0px;
    background-color: blue;
    float: left;
  }

  .polegar{
    margin: 10px;
    width: 100px;
    margin-right: -70%; 
    
  }
  .polegar-green{
    background: red;
  }
</style>
