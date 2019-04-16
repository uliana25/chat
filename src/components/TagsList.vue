<template>
  <div class="menu">
    <select id="tagslist" name="tags" size="3">
      <option 
        v-for="(item, index) in tagsList" 
        :key="index" 
        :style="item.style"
        @click.stop.prevent="hideTagsList(item.tag)"
      >{{item.tag}}</option>
    </select>
  </div>
</template>

<script>
import { system, list } from '../assets/serverData.json'

export default {
  name: 'TagsList',

  data(){
    return{
      system,
      list
    }
  },

  computed: {
    tagsList(){//берем все теги и формируем новый массив обьектов (у каждого обьекта есть свойство имя тега и цвет)
      let items = []
      this.system.map(x => {
        let item = {
          tag: "{{"+x+"}}",
          style: { background: "green"}
        }
        items.push(item)
      })
      this.list.map(x => {
        let item = {
          tag: "{{"+x+"}}",
          style: { background: "blue"}
        }
        items.push(item)
      })
      return items
    }
  },

  methods: {
    hideTagsList(tag) {//передаем по событию выбранный тег
      this.$emit('selectedTag',tag)
    }
  }
}
</script>

<style scoped>
.menu { 
  display:inline-block;
  vertical-align:top;
  border: none;
}
.menu select { 
  padding:0 px;
  margin:0
}
::-webkit-scrollbar {
    width: 12px;
}
 
::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3); 
    border-radius: 10px;
}
 
::-webkit-scrollbar-thumb {
    border-radius: 10px;
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.5); 
}
</style>
