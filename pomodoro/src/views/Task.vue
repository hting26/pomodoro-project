<template lang="pug">
#task
  b-container
    b-row
      b-col.mx-auto(cols="8")
        b-form-group(label-for="newinput" invalid-feedback="字數太少")
          b-form-input#newinput(
            placeholder="+ 新增待辦事項" v-model="newinput" :state="newinputstate" @keydown.enter="additem")
            //- :disabled="isBtnDisabled"
          b-btn#addbtn(pill @click="additem") 新增
      b-col.mx-auto.my-4(cols="8")
        b-tabs.mytabs(content-class='mt-3')
          b-tab.mytab(title='待辦事項' active)
            b-col.mx-auto.my-3
              b-table#todotable(:items="items" :fields="fields" show-empty)
                template(#empty)
                  div.emptybox
                    img(src="../assets/img_taskempty.svg")
                    p.text-center.my-5.empty 目前沒有待辦事項，<br>新增代辦事項並開始專注
                template(#cell(name)="data")
                  b-form-input(
                    v-if="data.item.edit"
                    v-model="data.item.model"
                    :state="data.item.state"
                    @keydown.enter="submitedit(data.index)"
                    @keydown.esc="canceledit(data.index)"
                  )
                  div.ml-4.todo(v-else) {{ data.value }}
                    img.playimg(src="../assets/icon_play.svg")
                    img.boximg(src="../assets/icon_checkbox_normal.svg")
                template(#cell(action)="data")
                  span(v-if="data.item.edit")
                    div.mybtn.mx-3
                      b-icon(icon="check" @click="submitedit(data.index)")
                    div.mybtn.mx-1
                      b-icon(icon="arrow-counterclockwise" @click="canceledit(data.index)")
                  span(v-else)
                    div.mybtn.mx-3.ml-auto
                      b-icon(icon="pencil" @click="edititem(data.index)")
                    div.mybtn.mx-1
                      b-icon(icon="trash" @click="delitem(data.index)")
                    .sort
                      | 排序
          b-tab.mytab.done(title='已完成事項')
            b-table-simple
                tr#donetask(v-for="(item, idx) in finished")
                  td {{ item }}
                  td
                    span.mybtn( @click="delfinish(idx)")
                      b-icon(icon="trash")
                tr(v-if="finished.length === 0")
                  img(src="../assets/img_taskempty.svg")
                  p.my-5.text-center.empty 目前沒有完成事項，<br>專注當下並完成代辦事項
</template>

<script>
export default {
  data () {
    return {
      newinput: '',
      fields: [
        { key: 'name' },
        { key: 'action' }
      ]
      // isBtnDisabled: true
    }
  },
  computed: {
    newinputstate () {
      return this.newinput.length > 2 ? true : this.newinput.length === 0 ? null : false
    },
    items () {
      return this.$store.state.items.map(item => {
        item.state = item.model.length > 2
        return item
      })
    },
    finished () {
      return this.$store.state.finished
    }
    // isBtnDisabled () {
    //   return this.newinputstate ? this.false : this.true
    // }
  },
  methods: {
    additem () {
      if (this.newinput.length > 2) {
        this.$store.commit('additem', this.newinput)
        this.newinput = ''
      }
    },
    edititem (index) {
      this.$store.commit('edititem', index)
    },
    delitem (index) {
      this.$store.commit('delitem', index)
    },
    submitedit (index) {
      if (this.items[index].state) {
        this.$store.commit('submitedit', index)
      }
    },
    canceledit (index) {
      this.$store.commit('canceledit', index)
    },
    delfinish (index) {
      this.$store.commit('delfinish', index)
    }
  }
}
</script>
<style lang="scss">
@import'../style/variables';

.container{
  width: 60%;
}
#task table>*{
    color: #68ABD7;
    }
#newinput{
  border: 2px solid #fff;
  background: transparent;
  border-radius: 50px;
  color: #68ABD7;
  }
#newinput:focus{
  border: 2px solid #fff;
  background: none;
  box-shadow: none;
}
#addbtn{
  width: 5rem;
  border: none;
  background: #C4DAE6;
  transition: .3s;
}
#addbtn:hover{
  filter: saturate(1.5);
}
#addbtn:focus{
  box-shadow: none;
  }
.mytabs .nav-link{
  background: none;
  color: #fff;
}
.mybtn{
  color: #fff;
  transition: .3s;
}
.mybtn:hover{
  cursor: pointer;
  background: none;
  border: none;
  color: #68ABD7;
}
.mybtn:focus{
  box-shadow: none;
  background: none;
  }
.mybtn:active{
  box-shadow: none;
  background: none;
  }
thead{
  text-align: left;
}
#todotable{
  text-align: left;
}
#todotable thead{
  display: none;
}
.empty{
  color: rgb(103, 171, 214);
  position: relative;
  top: -30PX;
}
span{
  display: flex;
  justify-content: space-between;
}
.mytabs .col{
  padding: 0;
}
.nav-tabs .nav-link.active{
  color: #fff;
  background-color: transparent;
}
#donetask{
  display: flex;
  justify-content: space-between;
}
#donetask div{
  display: none;
}
.done{
  margin-top: 1.8rem;
}
#newinput{
  padding-left: 1.5rem;
  height: 3rem;
}
#addbtn{
  position: absolute;
  top: .4rem;
  right: 1.4rem;
  height: 2.2rem;
}
.boximg{
  position: absolute;
  left: 0px;
  cursor: pointer;
}
.playimg {
  position: relative;
  left: 10px;
  bottom: 3px;
  width: 1.4rem;
  cursor: pointer;
}
.todo{
  border-bottom: 6px dotted;
  display: inline-block;
}
.b-table-empty-row{
  text-align: center;
  display: flex;
  justify-content: center;
}
.sort{
  text-decoration: underline;
  position: relative;
  top: calc(-70px + 1vw);
}
/* #addbtn:disabled{
  background: rgb(196, 218, 230);
  cursor: not-allowed;
} */
</style>
