<template lang="pug">
#app
  #nav
    #timer.h5.ml-auto
      router-link.text-white.navtool(to='/')
        b-icon-clock.mx-auto.h3
        | Timer
    #task.h5.mx-4
      router-link.text-white.navtool(to='/task')
        b-icon-check-square.mx-auto.h3
        | Task
    #settings.h5.mr-4
      router-link.text-white.navtool(to='/settings')
        b-icon-gear.mx-auto.h3
        | Settings
  router-view
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
