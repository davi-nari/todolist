<template>
    <transition name="modal">
        <div class="modal" @click="$emit('closeModal')">
            <div class="modal-content" @click.stop>
                <h2>{{ edit ? 'Изменить' : 'Добавить' }} заметку</h2>
                <form @submit.prevent="send">
                    <input type="text" required placeholder="Title" v-model="user.title"> 
                    <textarea placeholder="Content" v-model="user.descr"></textarea>
                    <div class="modal-btns">
                        <a @click.prevent="$emit('closeModal')" href="#">ОТМЕНА</a>
                        <button v-if="!edit">ДОБАВИТЬ</button>
                        <button @click="changeNote" type="button" v-else>ИЗМЕНИТЬ</button>
                    </div>
                </form>
            </div>
        </div>
    </transition>
</template>

<script>
    export default {
        props: ['currentId', 'editObj', 'edit'],
        data(){
            return{
                user: {
                    title: '',
                    descr: '',
                    id: this.currentId
                }
            }
        },
        methods:{
            send(){
                this.$emit('addNote', {...this.user})
                this.$emit('closeModal')
                for (const key in this.user) this.user[key] = ''
            },
            changeNote(){
                if(this.title != '' && this.descr != ''){
                    const editNote = {
                        id: this.editObj.id,
                        title: this.user.title,
                        descr: this.user.descr,
                        date: this.editObj.date
                    }
                    this.$emit('editedNote', editNote)  
                    this.$emit('closeModal')              
                }
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>