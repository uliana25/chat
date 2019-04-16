<template>
  <div id="app">
    <textarea id="mytextarea" name="message" rows="10" cols="30" v-model="message"/>
    <tags-list v-show="showSelect" @selectedTag="addTag"/>
  </div>
</template>

<script>
import TagsList from './TagsList.vue'

export default {
  /* eslint-disable */
  name: 'chat',

  components: {
    TagsList
  },

  data(){
    return{
      message: '',
      showSelect: false,
      currentPos: 0
    }
  },

  watch: {
    // eslint-disable-next-line
    'message' () {//следим за изменением сообщения
      this.switchVisibilityOfSelect()
      this.saveMessage()
    }
  },

  mounted() {
    this.message = localStorage.getItem('message') || '' //берем значение message из LocalStorege на mounted, 
    //что бы компонент уже имплементировался и переменные из data были доступны
  },

  methods:{
    saveMessage(){ //сохраняем сообщение и проверяем на превышение памяти стореджа
      try {
        localStorage.setItem('message', this.message);
      } catch (e) {
        if (e == QUOTA_EXCEEDED_ERR) {
        alert('Превышен лимит');
        }
      }
    },

    async addTag(tag){ //вставляем тег в нужное место. async/await -потому что в DOM все происходит асинхронно
      await (this.message = this.message.slice(0, this.currentPos) + tag.slice(2) + this.message.slice(this.currentPos))
      await document.getElementById('mytextarea').focus()
      let pole = document.getElementById('mytextarea')
      let pos=(this.message.slice(0, this.currentPos) + tag.slice(2)).length
      
      await (pole.selectionStart = pos)// ставим курсор в новую позицию после вставленного тега
      await (pole.selectionEnd = pos)
      this.switchVisibilityOfSelect( ) 
    },

    async switchVisibilityOfSelect () {// берет два элемента влево от позиции курсора, и если они совпадают с {{ то делает видимым 
    //селект с тегами
      let ctrl = document.getElementById('mytextarea')
      if (!ctrl) return
      // eslint-disable-next-line
      if ( ctrl.selectionStart || ctrl.selectionStart == '0' ) {
          this.currentPos = ctrl.selectionStart
      }
      let pieceOfMessege = this.message.slice(this.currentPos-2, this.currentPos)
      this.showSelect = pieceOfMessege.match(/\{{2}/g)
      if (this.showSelect===null){
        this.showSelect = false
      } else {
        this.showSelect = true
      }
    }
  }
}
</script>

<style>
textarea {
	/* = Убираем скролл */
	overflow: auto;
  position: relative;
	/* = Убираем увеличение */
	resize: none;
	width: 300px;
	height: 50px;

	/* = Добавим фон, рамку, отступ*/
	background: #f6f6f6;
	border: 1px solid #cecece;
	border-radius: 5px;
	padding: 8px 0 8px 10px;
  /* = Убираем синюю обводку*/
  outline: none;
	-moz-appearance: none;
}
</style>
