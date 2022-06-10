<template>  
  <div id="play" class="border"> 
    <img src="../assets/bam.png" alt="" style="width:440px" v-if="!isPlay()">

    <audio controls ref="audio" hidden>
      <source src="../audio/clap2.mp3" type="audio/mpeg" />
    </audio>
    <audio autoplay="autoplay" ref="audio" v-if="!start" >
      <source src="../audio/TrapPesado.wav" type="audio/mpeg" />
    </audio>

    <b>BASIC ABOUT MUSIC</b>
    <hr><hr><hr>

    <div>      
      <div id="pulse" class="counter" v-if="isPlay()">
        <div v-for="pulse in pulses" :key="pulse.value"  class="pulse" :class="{yellowbg : pulse.value == count}">
          {{pulse.value}} 
        </div>
      </div>
    </div>
    <div id="view" class="item-view" v-if="isPlay()">      
        <div v-for="(item, index) in compasso.value" :key="index" :class="{bg4: item == 4, bg2: item == 2, bg1: item == 1}" :style="getWidthImg(item)">
          <img :src="getImgUrl(item)" /><br>
          <h1>{{item}}</h1>      
        </div>          
    </div>
    <div>
      <button @click="play" :class="{'bg-stop' : isPlay()}">{{botao}}</button>  
      <TouchBody v-if="isPlay()" :play="isPlay()" @new-short="short" :shorted="compasso.value" :last="count"/>   
    </div>
  </div>
</template>

<script>
import TouchBody from '../components/TouchBody';

export default {
  name: "Play",
  components:{
    TouchBody
  },
  data(){
    return{
      pulses: [
        {desc: 'first' , value:1},
        {desc: 'secondi' , value:2},
        {desc: 'third' , value:3},
        {desc: 'fourth' , value:4}
      ],       
      compassos: [
           {value: [4] },
           {value: [2,2] },
           {value: [1,2,1] },
           {value: [1,1,2] },
           {value: [2,1,1] },
           {value: [1,1,1,1]},
      ], 
      compasso: [],
      timer: null,
      botao: 'Start',
      count: 0,
      isShort: true,
      start: false,
      audio: null,
    }
  },

  methods:{ 
    isPlay(){
      return this.start;
    }, 
    playClap(){         
        this.audio.pause();
        this.audio.currentTime = 0;          
        this.audio.play();        
     },
    play(){ 
       this.short();  
       this.start = true;
       let music = document.getElementById('play1');
       if(this.timer !== null){
         //AQUI TEM ALGO RODANDO NO TIMER...
         clearInterval(this.timer);
         this.timer = null;
         this.botao = 'PLAY'; 
         this.start = false; 
         this.count = 0; 
         this.compasso = [];       
       }else{                       
         //O TIMER ESTÁ ZERADO OU PARADO
         if(this.count > 4) this.count = 1;
         this.timer = setInterval(() =>{
         this.setCount()
         }, 1000); //1 SEGUNDO = 1000 MILESEGUNDOS
         this.botao = 'STOP';        
       }         
     },
     setCount(){    
       this.playClap();          
       this.count >= 4 ? this.count = 1 : this.count++;
       //console.log(this.count);
       
     },
     short(){       
       this.compasso = this.compassos[Math.floor(Math.random() * this.compassos.length)];
       this.isShort = false;
       //console.log(this.compasso, 'this.compasso');       
     },
     getImgUrl(img){       
       return require('../assets/'+img+'.png');
     },     
     getWidthImg(img){
       if(img == 4) return "width: 100%;"; 
       if(img == 2) return 'width: 50%; heigth: 100%';
       if(img == 1) return 'width: 25%;';
     }
  },
  mounted(){
    this.audio = document.querySelector('audio'); 
    if(this.isShort){
      this.short();
      console.log('Sorteou');
    }     
  }
};
</script>

<style scoped>
  button{
    width: 100%;  
    height: 50px;    
    background-color: black;   
    border-radius: 10px; 
    color: white;
  }
  button:hover{
    background-color: black;
    color: yellow;
    font-size: 30px;
    
  }
  .border {
    height: 100%;
    border: 5px solid black;
    padding: 10px;    
    background-color: lime;   
  }
  .counter{
    border: 2px solid black;
    display: flex;
    flex-direction: row;     /* make main axis horizontal (default setting) */
    justify-content: center; /* center items horizontally, in this case */
    align-items: center;     /* center items vertically, in this case */ 
    background: white;
  }
  
  .item-view{    
    display: flex;
    flex-direction: row;     /* make main axis horizontal (default setting) */
    justify-content: center; /* center items horizontally, in this case */
    align-items: center;     /* center items vertically, in this case */ 
  }
  .pulse{
    width: 25%;
    text-align: center;
    font-size: 20px;
    border-right-style: solid;    
  }  
  .yellowbg{
    background-color: yellow;    
  }
  .bg4{
    border: 0.3px solid white;
    background-color: red;  
    border-radius: 5px;     
    
  }
   .bg2{
    border: 0.3px solid white;
    background-color: purple;
     color: white;    
  }
   .bg1{    border: 1px solid white;
    background-color: blue;  
    color: white;  
  }
  .bg-stop{
      border: 0.5px solid white;
      background-color: red;  
      color: white;          
   }
  .item-view{
    opacity: 0.8;
  }
  .hidden{
    visibility: hidden;
  }
  .action button{
    width: 48%;  
    margin-top: 10px;
    border-color: grey;
  }
</style>