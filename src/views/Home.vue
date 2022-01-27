<template lang="pug">
#home
  b-container
    b-row
      b-col(cols="12")
        h1.mt-1.mb-5 {{ currentText }}
        p.timer.my-3 {{ timeText }}
        .btngroup
          button.playbtn.my-4.mx-auto(v-if="status !== 1" @click="start")
            img(:src="pictureHover" @mouseover="hover = true" @mouseleave="hover = false")
          div(v-else)
            button.pausebtn.my-4( @click="pause")
              img(src="../assets/icon_pause.svg")
            .reset.my-2
              | Reset
        .dot
          button.donebtn.mx-auto(v-if="current.length > 0" @click="finish(true)")
            | 完成此代辦
  div
    .line.my-5(:style="{width: lineWidth}")
      .focus| Focus......
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
      hover: false,
      playhover: require('../assets/icon_play_hover.svg'),
      playimg: require('../assets/icon_play.svg')
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
    },
    pictureHover () {
      if (this.hover === true) {
        return this.playhover
      } else {
        return this.playimg
      }
    },
    lineWidth () {
      const timeline = (
        this.timeleft / (this.$store.state.break === false ? parseInt(process.env.VUE_APP_TIME) : parseInt(process.env.VUE_APP_TIMEBREAK))) * 100
      return timeline + 'vw'
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
  overflow: hidden;
}
.container{
  text-align: center;
  height: 360px;
}
.row{
  margin-top: 3vw;
}
.timer{
  font-size: 4.5rem;
}
.btngroup{
  display: flex;
  flex-direction: column;
}
.donebtn{
  border: 1px solid #fff;
  border-radius: 50px;
  width: 130px;
  height: 3rem;
  line-height: 2.2rem;
  color: #68ABD7;
  position: relative;
  top: 50px;
  transition: .3s;
}
.donebtn:hover{
  color: #fff;;
}
.line{
  position: relative;
  width: 100vw;
  height: 1px;
  background-color: #fff;
  transition: 1s;
}
.focus{
  position: absolute;
  right: -70px;
  top: -11px;
  color: #fff;
}
button{
  background: transparent;
  border: none;
}
.reset{
  cursor: pointer;
}
.dot{
  position: absolute;
  width: 200px;
  border-top: 10px dotted;
  margin: 0 auto;
  left: 50%;
  transform: translateX(-50%);
  top: calc(500px + 3vh);
  /* bottom: -200px; */
}
</style>
