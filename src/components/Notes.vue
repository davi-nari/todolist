<template>
    <div class="notes">
        <div class="container">
            <div class="notes-nav">
                <h2>{{ notes.length ? 'Все заметки' : 'Нет заметок' }}</h2>
                <button @click="grid = !grid">
                    <img v-if="grid" src="@/assets/img/grid.svg" alt="">
                    <img v-else src="@/assets/img/list.svg" alt="">
                    <span>{{ grid ? "Сетка" : 'Список' }}</span>
                </button>
            </div>
            <transition-group tag="div" class="notes-grid" name="notes" :class="{columns: grid}">
                <NoteItem v-for="(note, index) in notes" :key="index"
                 :note="note" 
                 @deleteNote="$emit('deleteNote', note.id)"
                 @changeNote="$emit('changeNote', note.id)"
                 />
            </transition-group>
        </div>
    </div>
</template>

<script>
import NoteItem from './NoteItem.vue'
export default {
    components: { NoteItem },
    props: ['notes'],
    data(){
            return{
                grid: false
            }
        }
    }
</script>

<style>

</style>