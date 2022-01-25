<template lang="pug">
#settings
  b-container
    b-row
      b-col(cols="12")
        b-table(:items="items" :fields="fields"  @row-clicked='select')
          template(#cell(src)="data")
            audio(controls :src="require('@/assets/' + data.value)")
          template(#cell(select)="data")
            span(v-if="data.item.src === sound") <b-icon-check class="h3"></b-icon-check>
</template>
<script>
export default {
  data () {
    return {
      items: [
        { name: 'saxsoloe', src: 'saxsoloe.wav' },
        { name: 'organ melancholic progression', src: 'organ melancholic progression.wav', select: '' }
      ],
      fields: [
        { key: 'name', label: '鈴聲' },
        { key: 'src', label: '試聽' },
        { key: 'select', label: '選擇' }
      ]
    }
  },
  methods: {
    select (item) {
      this.$store.commit('selectSound', item.src)
    }
  },
  computed: {
    sound () {
      return this.$store.state.sound
    }
  }
}
</script>

<style>

.container{
  width: 50%;
}
#settings table>*{
  color: #68ABD7;
  text-align: left;
}
#settings table tr{
  cursor: pointer;
}
#settings tbody tr td:nth-child(1),
#settings tbody tr td:nth-child(3){
  position: relative;
  top: 18px;
}
</style>
