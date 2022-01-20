<template>
  <b-container>
    <h1>This is an timer page</h1>
    <h1>This is an timer page</h1>
    <h1>{{ currentText }}</h1>
    <h2>{{ timeText }}</h2>
    <b-button class="timerbtn"><b-icon-play></b-icon-play></b-button>
  </b-container>
</template>

<script>
</script>
<!-- <template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld
  }
}
</script> -->
<style>
.container{
  text-align: center;
}
.timerbtn{
  border: none;
  background: transparent;
  color: #68ABD7;
  font-size: 2rem;
}
.timerbtn:hover{
  border: none;
  background: transparent;
  color: #fff;
}
</style>
