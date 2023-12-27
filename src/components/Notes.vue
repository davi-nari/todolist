<template>
    <div class="notes">
        <div class="container">
            <div class="notes-nav">
                <h2 v-if="search.length === 0">{{ notes.length ? words.infobar[lang] : words.noinfobar[lang] }}</h2>
                <h2 v-if="search && notes.length > 0 ">{{words.infobar[lang]}}</h2>
                <h2 v-if="search && notes.length === 0 ">{{words.nothingfound[lang]}}</h2>
                <button @click="grid = !grid">
                <img src="@/assets/img/grid.svg" alt="" v-if="grid">
                <img src="@/assets/img/list.svg" alt="" v-else>
                <span>{{ grid ? words.grid[lang] : words.list[lang] }}</span>
                </button>
            </div>
            <TransitionGroup tag="div" class="notes-grid" name="notes" :class="{column: grid}">
                <NoteItem v-for="(note, index) in notes" :key="index" 
                          :note="note"
                          :lang="lang"
                          :grid="grid"
                          @delNote="$emit('delNote', note.id, note.title)"
                          @changeNote="$emit('changeNote', note.id)"
                          />
            </TransitionGroup>
        </div>
    </div>
</template>

<script>
import NoteItem from './NoteItem.vue'
    export default {
        components: { NoteItem },
        props:['notes', 'search', 'lang'],
        inject: ['words'],
        data() {
            return {
                grid: false
            }
        },
        methods: {
            screenMedia(){
                if(window.matchMedia('(max-width: 696px)').matches){
                    this.grid = true
                }
            }
        },
        created() {
            this.screenMedia()
        },
    }
</script>

<style>

</style>