<template lang="pug">
#home
  b-container
    b-row
      b-col(cols="12")
        h1 {{ currentText }}
        p.timer {{ timeText }}
        .btngroup.my-4
          btn.playbtn.my-2(v-if="status !== 1" @click="start"
          @mouseover="hover = true" @mouseleave="hover = false")
            img(v-show="hover" src="../assets/icon_play.svg")
            img(v-hide="hover=false" src="../assets/icon_play_hover.svg")
          btn.my-2(v-else @click="pause")
            img(src="../assets/icon_pause.svg")
          btn.donebtn.mx-auto.my-2(v-if="current.length > 0" @click="finish(true)")
            | 完成此代辦
</template>

<script>
export default {
  data () {
    return {
      // 0 = 停止
      // 1 = 倒數中
      // 2 = 暫停
      status: 0,
      timer: 0,
      hover: false
    }
  },
  computed: {
    current () {
      return this.$store.state.current
    },
    items () {
      return this.$store.state.items
    },
    currentText () {
      return this.current.length > 0 ? this.current : this.items.length > 0 ? '點擊開始' : ''
    },
    timeleft () {
      return this.$store.state.timeleft
    },
    timeText () {
      const m = Math.floor(this.timeleft / 60).toString().padStart(2, '0')
      const s = Math.floor(this.timeleft % 60).toString().padStart(2, '0')
      return `${m} : ${s}`
    }
  },
  methods: {
    start () {
      if (this.status === 0 && this.items.length > 0) {
        this.$store.commit('start')
      }
      if (this.current.length) {
        this.status = 1
        this.timer = setInterval(() => {
          this.$store.commit('countdown')
          if (this.timeleft <= -1) {
            this.finish(false)
          }
        }, 1000)
      }
    },
    pause () {
      this.status = 2
      clearInterval(this.timer)
    },
    finish (skip) {
      clearInterval(this.timer)
      this.status = 0
      this.$store.commit('finish')

      if (!skip) {
        const audio = new Audio()
        audio.src = require('@/assets/' + this.$store.state.sound)
        audio.play()
      }

      if (this.items.length > 0) {
        this.start()
      }
    }
  }
}
</script>

<style>
body{
  font-family: 'Cinzel', serif;
}
.container{
  text-align: center;
}
.row{
  margin-top: 8vw;
}
.timer{
  font-size: 4.5rem;
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
.btngroup{
  display: flex;
  flex-direction: column;
}
btn{
  cursor: pointer;
}
.donebtn{
  border: 1px solid #fff;
  border-radius: 50px;
  width: 130px;
  height: 35px;
  line-height: 35px;
}
</style>
