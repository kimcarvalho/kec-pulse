<template> 
  <div class="home">
     <audio autoplay="autoplay" loop="true" id="music" ref="audio">
       <source src="../audio/bam.mp3" type="audio/mpeg" />
     </audio>
    <h2>GAMIFICATION</h2>
    <img alt="Vue logo" src="../assets/muzk.png" />
    <KecPulse />
  </div>
</template>

<script>
// @ is an alias to /src
import KecPulse from "@/components/KecPulse.vue";

export default {
  name: "Home",
  components: {
    KecPulse,
  }, 
  mounted(){
    //document.getElementById("music").play()
  }, 
};
</script>

<style scoped>
  h2{
    margin-top: 20%;
  }
 
</style>
           