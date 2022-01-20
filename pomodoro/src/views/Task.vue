<template lang="pug">
#task
  b-container
    b-row
      b-col.mx-auto(cols="6")
        b-form-group(label-for="newinput" invalid-feedback="字數太少")
          b-form-input#newinput(placeholder="+ 新增代辦事項" v-model="newinput" :state="newinputstate" @keydown.enter="additem")
        b-btn#addbtn(pill, @click="additem") 新增
      b-col.my-3(cols="12")
        b-table(:items="items" :fields="fields" show-empty)
          template(#empty)
            p.text-center 沒有項目
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
              b-btn.mx-1(variant="success")
                b-icon(icon="check" @click="submitedit(data.index)")
              b-btn.mx-1(variant="danger")
                b-icon(icon="arrow-counterclockwise" @click="canceledit(data.index)")
            span(v-else)
              b-btn.mx-1
                b-icon(icon="pencil" @click="edititem(data.index)")
              b-btn.mx-1
                b-icon(icon="trash" @click="delitem(data.index)")
      b-col(cols="12")
        h1.text-center 已完成
      b-col(cols="12")
        b-table-simple
          thead
            th 名稱
            th 操作
          tbody
            tr(v-for="(item, idx) in finished")
              td {{ item }}
              td
                b-btn(variant="danger" @click="delfinish(idx)")
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
        { key: 'action', label: '操作' }
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
  }
</style>
