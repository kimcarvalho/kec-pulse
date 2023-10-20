<template>
    <div class="modal">
        <div class="container">
            <a class="close" @click="close()">Retry</a>
            <div>
                <h1><b>Final Record</b> {{ tentativa }}</h1>
                <hr>

                <div class="content">
                    <div>
                        <h2>{{hits}}</h2>
                        <h3>hits</h3>                        
                    </div>
                    <div>
                        <h2>16</h2>
                        <h3>Attempts</h3>                        
                    </div>
                    <div>
                        <h2>{{errors}}</h2>
                        <h3>Errors</h3>                        
                    </div>
                </div>
                <hr>
                <img :src="getImgUrl()" :title="title" alt="Colocação">
                <p>
                    <h4>{{title}}</h4>
                </p>
            </div>
        </div>
    </div>
  </template>
  
  <script>
  export default {
      name: 'ModalFinal',
      data(){
        return{
            medal: "3th.png",
            title: "Classification"
        }
      },
      props: {        
        errors: Number,
        hits: Number,
        play: Boolean,
        tentativa: Number,
        show: false
      },
      watch:{
            show(val){
                if(val === true){
                    console.log("Entrou aqui");
                    
                }
            }         
        },
      methods:{
        close(){
          this.$emit('close-modal');            
        },
        getImgUrl(){     
            if(this.hits >= this.errors){
               if(this.hits >= 8 && this.hits < 12){
                    console.log("Entrou aqui 3th");
                    this.medal = "3th.png";
                    this.title = "3th - Colocation"
               }else if(this.hits >= 12 && this.hits < 15){
                    this.medal = "2nd.png";
                    this.title = "2nd - Colocation";
                    console.log("Entrou aqui 2nd");
               }else if(this.hits >= 15 && this.hits <= 16){
                    this.medal = "1st.png";
                    this.title = "1st - Colocation"
                    console.log("Entrou aqui 1 st");
               }else{
                    this.medal = "failed.png"
                    this.title = "Sorry - Try again!"
                    console.log("Entrou aqui errou");
               }
            } 

            return require('../assets/'+this.medal);
        }, 
      },      
  }
  </script>
  
 <style scoped>
    
    .content{        
        display: flex!important;
        width: 100%;
        background-color: black;
        padding: 9px;
        margin: 2px;
        color: black;    
    }

    .content div{
        display: table-row;
        width: 30%;
        background: white;
    }
      .modal{
          position: fixed;
          top:0;
          right: 0;
          bottom: 0;
          left: 0;
          background: rgba(0,0,0,0.5);
          z-index: 999; 
      }
  
      .modal .container{
          position: fixed;
          max-width: 400px;
          top: 4vh;
          bottom: 4vh;
          left: 0;
          right: 0;
          margin: 0 auto;
          padding: 2rem;
          padding-left: 10px;
          border-radius: 4px;
          background-color: #FFF;
          box-shadow: 0 0 20 rgba(0,0,0,0.7);
          overflow: auto;
      }
  
      .close{
          cursor: pointer;
          background: #121212;
          padding: 4px 15px;
          color: #FFF;
          position: absolute;
          top: 10px;
          left: 10px;
      }
  
      span{
          font-style: italic;
          font-weight: bold;
          color: #121212;
      }
  
      p{
          white-space: pre-wrap;
      }

      img{
        width: 150px;        
      }
  </style>
  