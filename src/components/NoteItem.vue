<template>
    <div class="notes-item">
        <h3>{{ note.title }}</h3>
        <span>{{ note.date }}</span>
        <p class="notes-item-descr">
            {{ note.descr }}
        </p>
        <div class="notes-item-controls">
            <a @click.prevent="$emit('changeNote',note.id)" href="#">
                <img src="@/assets/img/edit.svg" alt="">
                РЕДАКТИРОВАТЬ
            </a>            
            <a @click.prevent="$emit('deleteNote',note.id)" href="#">
                <img src="@/assets/img/d.svg" alt="">
                УДАЛИТЬ
            </a>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['note']
    }
</script>

<style lang="scss" scoped>

</style>