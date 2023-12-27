<template>
    <div class="notes-item">
        <h3>{{note.title}}</h3>
        <span>{{note.date}}</span>
        <p>{{note.descr}}</p>
        <div class="notes-item-controls">
            <a href="#" class="btn" @click.prevent="$emit('changeNote', note.id)">
                <img src="@/assets/img/pen.svg" alt="">{{screen ? words.editbtn[lang] : ''}}
            </a>
            <a href="#" class="btn" @click.prevent="$emit('delNote', note.id, note.title)">
                <img src="@/assets/img/trash.svg" alt="">{{screen ? words.deledit[lang] : ''}}
            </a>
        </div>
    </div>
</template>

<script>
    export default {
        inject: ['words'],
        props: ['note', 'lang', 'grid'],
        data() {
            return {
                screen: true
            }
        },
        methods: {
            screenMedia(){
                if(window.matchMedia('(max-width: 696px)').matches && !this.grid){
                    this.screen = false
                }
            }
        },  
        created() {
            this.screenMedia()
        },
    }
</script>

<style lang="scss" scoped>

</style>