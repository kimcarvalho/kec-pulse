<template>
  <div class="kecpulse">
    <h1>XP | UX </h1>       
    <div id="nav">
      <router-link to="/play"><h1>PLAY</h1></router-link> 
      <!-- <router-link to="/test">Test</router-link> -->
    </div>
  </div>
</template>

<script> 

export default {
  name: "KecPulse",  
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #nav {
    padding: 30px;
  }

  #nav a {
    font-weight: bold;
    font-size: 15px;
    color: black;
  }

  #nav a.router-link-exact-active {
    color: #42b983;
  }
</style>
