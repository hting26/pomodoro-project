<template lang="pug">
#app
  #nav.pt-5.pr-5
    #timer.h6
      router-link.navtool(to='/')
        b-icon-clock.mx-auto.h4
        | Timer
    #task.h6
      router-link.navtool(to='/task')
        b-icon-check-square.mx-auto.h4
        | Task
    .h6
      a.navtool(href='#')
        b-icon-bar-chart-line-fill.mx-auto.h4
        | Report
    #set.h6
      router-link.navtool(to='/settings')
        b-icon-gear.mx-auto.h4
        | Settings
  router-view
</template>

<style>
body{
  background: linear-gradient(45deg, #C4DAE6,#E1E6E6);
    color: #68ABD7;
    height: 100vh;
  }
#nav{
  display: flex;
  padding: 1rem;
  justify-content: flex-end;
  }
.navtool{
  display: flex;
  flex-direction: column;
  justify-content: center;
  color: #fff;
  transition: .3s;
  margin: 0 .8rem;
  }
.navtool:hover{
  color: rgb(146, 200, 224);
  text-decoration: none;
  }
.router-link-exact-active{
  color: rgb(146, 200, 224);
  }

</style>
