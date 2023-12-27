<template>
<Navbar   
       @searchValue="search = $event"
       @changeLang="changeLang"
       :lang="lang"
       :showModal="showModal"
       :smallScreen="smallScreen"
       @closeModal="showModal = false"
       :edit="edit"
/>

<main v-if="smallScreen">
  <Transition name="modalWin" mode="out-in">
    <Notes v-if="!smallScreen || (smallScreen && !showModal)"
          :notes="filterNotes"
          @delNote="delNote"
          @changeNote="changeNote"
          :search="search"
          :lang="lang"
    />

    <Modal 
      v-else 
      @closeModal="showModal = false"
      @addNote="addNote"
      :editNote="editNote"
      :edit="edit"
      @editedNote="editedNote"
      :lang="lang"
      :smallScreen="smallScreen"
    />
  </Transition>
</main>

<main v-else>
    <Notes
        :notes="filterNotes"
        @delNote="delNote"
        @changeNote="changeNote"
        :search="search"
        :lang="lang"
  />

  <Modal 
    v-show="showModal"
    @closeModal="showModal = false"
    @addNote="addNote"
    :editNote="editNote"
    :edit="edit"
    @editedNote="editedNote"
    :lang="lang"
    :smallScreen="smallScreen"
  />
</main>

<DelModal 
  v-show="showDel"
  :lang="lang" 
  :delTitle="delTitle"
  @closeModal="showDel = false"
  @deleteNote="deleteNote"/>

<Transition name="addNote-anim">
  <a href="#" class="addNote" 
    v-show="!smallScreen || (smallScreen && !showModal)" 
    @click.prevent="showModal = true, edit = false">
    <img src="@/assets/img/pen.svg" alt=""></a>
</Transition>
</template>

<script>
import langs from './lang'
import Modal from './components/Modal.vue'
import Navbar from './components/Navbar.vue'
import Notes from './components/Notes.vue'
import DelModal from './components/DelModal.vue'
  export default {
    components: { Navbar, Notes, Modal, DelModal },
    data() {
      return {
        showModal: false,
        notes: [],
        currentId: localStorage.id ? JSON.parse(localStorage.id) : 0,
        edit: false,
        editNote: {},
        search: '',
        lang: 'ru',
        langwords: {},
        smallScreen: false,
        delTitle: '',
        delId: '',
        showDel: false
      }
    },
    methods: {
      addNote(obj){
        const note = {...obj} //чтобы мы смогли работать с приёмным объектом
        note.date = new Date().toLocaleString()
        note.id = this.currentId++ //сначала пртсваивает, потом увеличивает 
        this.notes.push(note)
      },
      getNotes(){
        const localNotes = localStorage.notes
        if(localNotes){
          this.notes = JSON.parse(localNotes)
        }
      },
      changeNote(id){
        this.showModal = this.edit = true
        let pickedNote = this.notes.find(note => id === note.id)
        this.editNote = pickedNote //переменную примваем как знач ключа, чтобы пропсом передать
      },
      editedNote(editNote){
        this.notes.forEach(note => {
          if(note.id == editNote.id){
            note.title = editNote.title
            note.descr = editNote.descr
            note.date = editNote.date
          }  
        })
      },
      delNote(id, title){
        this.delTitle = title
        this.delId = id
        this.showDel = true
      },
      deleteNote(){
        let index = this.notes.findIndex(note => this.delId === note.id)
        this.notes.splice(index, 1)
        this.currentId--
        this.showDel = false
      },
      changeLang(val){
        this.lang = localStorage.lang = val
      },
      screenMedia(){
        if(window.matchMedia('(max-width: 696px)').matches){
          this.smallScreen = true
        }
      }
    },
    computed: {
      filterNotes(){
        if (this.search){
          return this.notes.filter(note =>
            note.descr.toLowerCase().includes(this.search.toLowerCase()) || note.title.toLowerCase().includes(this.search.toLowerCase())
          )
        } return this.notes
      }
    },
    created(){
      this.screenMedia()
      this.getNotes()
      localStorage.lang = localStorage.lang || 'ru'
      this.lang = localStorage.lang || 'ru'
      this.langwords = langs
      localStorage.words = JSON.stringify(this.langwords)
    },
    provide(){
      return {
        searchValue: this.search,
        words: localStorage.words ? JSON.parse(localStorage.words) : langs
      }
    },
    watch: {
      notes: {
        handler(newNotes){
          localStorage.notes = JSON.stringify(newNotes)
        },
        deep: true
      },
      currentId: {
        handler(savedId){
          localStorage.id = JSON.stringify(savedId)
        },
        deep: true
      }
    }
  }
</script>

<style>

</style>