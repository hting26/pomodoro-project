<template>
  <div id="app">
    <div id="nav">
      <div id="timer" class="h5 ml-auto">
        <router-link to="/" class="text-white navtool"><b-icon-clock class="h2"></b-icon-clock>Timer</router-link>
        </div>
      <div id="task" class="h5 mx-5">
        <router-link to="/task" class="text-white navtool"><b-icon-check-square class="h2"></b-icon-check-square>Task</router-link>
        </div>
      <div id="settings" class="h5 mr-5">
        <router-link to="/settings" class="text-white navtool"><b-icon-gear class="h2"></b-icon-gear>Settings</router-link>
        </div>
    </div>
    <router-view></router-view>
  </div>
</template>

<style lang="stylus">
body
  background linear-gradient(45deg, #C4DAE6,#E1E6E6)
  color #68ABD7
  height 100vh
#app
  // font-family Avenir, Helvetica, Arial, sans-serif
  // -webkit-font-smoothing antialiased
  // -moz-osx-font-smoothing grayscale
  // text-align center
  // color #2c3e50
  // margin-top 60px
#nav
  // background #000
  display flex
  padding 1rem
.navtool
  display flex
  flex-direction column
  justify-content center
</style>
<!-- <style>
// .navtool{
//   justify-content: center;
// }
// </style> -->
