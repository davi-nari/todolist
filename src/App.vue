<template>
  <Navbar />
  <Notes 
  :notes="notes" 
  @deleteNote="deleteNote"
  @changeNote="changeNote"
  />
  <Modal 
  v-show="showModal" 
  @closeModal="showModal = false"
  @addNote="addNote"
  :currentId="currentId"
  @deleteNote="deleteNote"
  :editObj="editObj"
  :edit="edit"
  @editedNote="editedNote"
  />
  <a href="#" class="addNote" @click.prevent="showModal = true, edit = false">
    <img src="@/assets/img/edit.svg" alt="">
  </a>
</template>

<script>
import Modal from './components/Modal.vue'
import Navbar from './components/Navbar.vue'
import Notes from './components/Notes.vue'

export default {
  components: { Navbar, Notes, Modal },

  data(){
    return{
      showModal: false,
      notes: [],
      currentId: localStorage.noteId ? JSON.parse(localStorage.noteId) : 0,
      edit: false,
      editObj: {}
    }
  },

  methods: {
    addNote(obj){
    const note = {...obj}
    note.date = new Date().toLocaleString()
    note.id = this.currentId++
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
      let pickedNote = this.notes.find(note => note.id === id)
      this.editObj = pickedNote
    },
    editNote(noteEdited){
      this.notes.forEach(note => {
        if(note.id == noteEdited.id){
          note.title = noteEdited.title
          note.descr = noteEdited.descr
          note.date = noteEdited.date
        }
      })
    }
  },
  created(){
    this.getNotes()
  },
  watch: {
    notes: {
      handler(newNotes){
        localStorage.notes = JSON.stringify(newNotes)
        localStorage.noteId = JSON.stringify(this.currentId)
    },
      deep: true
    }
  }
  }

</script>

<style lang="scss" scoped>

</style>