<template lang="pug">
#task
  b-container
    b-row
      b-col.mx-auto(cols="6")
        b-form-group(label-for="newinput" invalid-feedback="字數太少")
          b-form-input#newinput(
            placeholder="+ 新增待辦事項" v-model="newinput" :state="newinputstate" @keydown.enter="additem")
        b-btn#addbtn(pill, @click="additem") 新增
      b-col.mx-auto(cols="8")
        p.title.text-center 待辦事項
      b-col.mx-auto.my-3(cols="8")
        b-table(:items="items" :fields="fields" show-empty)
          template(#empty)
            p.title.text-center 沒有項目
          template(#cell(name)="data")
            b-form-input(
              v-if="data.item.edit"
              v-model="data.item.model"
              :state="data.item.state"
              @keydown.enter="submitedit(data.index)"
              @keydown.esc="canceledit(data.index)"
            )
            span(v-else) {{ data.value }}
          template(#cell(action)="data")
            span(v-if="data.item.edit")
              btn.mybtn.mx-3
                b-icon(icon="check" @click="submitedit(data.index)")
              btn.mybtn.mx-1
                b-icon(icon="arrow-counterclockwise" @click="canceledit(data.index)")
            span(v-else)
              btn#editbtn.mybtn.mx-3
                b-icon(icon="pencil" @click="edititem(data.index)")
              btn#delbtn.mybtn.mx-1
                b-icon(icon="trash" @click="delitem(data.index)")
      b-col.mx-auto(cols="8")
        p.title.text-center 已完成事項
      b-col.mx-auto(cols="8")
        b-table-simple
          thead
            th 名稱
            th 操作
          tbody
            tr(v-for="(item, idx) in finished")
              td {{ item }}
              td
                btn.mybtn( @click="delfinish(idx)")
                  b-icon(icon="trash")
            tr(v-if="finished.length === 0")
              td.text-center(colspan="2") 沒有項目
</template>

<script>
export default {
  data () {
    return {
      newinput: '',
      fields: [
        { key: 'name', label: '名稱' },
        { key: 'action', label: '' }
      ]
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
<style>
/* #task{
  text-align: center;
} */
#task table>*{
    color: #68ABD7;
    }
#task table> tr{
  text-align: left;
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
  background: rgb(146, 200, 224);
  opacity: 0.3;
  transition: .3s;
  }
#addbtn:hover{
  opacity: 1;
  }
#addbtn:focus{
  box-shadow: none;
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
.title{
  color: #fff;
}
</style>
