<template>
    <Transition name="modal" v-if="!smallScreen">
        <div class="modal" @click="$emit('closeModal')">
            <div class="modal-content" @click.stop>
                <h2>{{edit ? words.titlewindowedit[lang] : words.titlewindow[lang]}}</h2>
                <form @submit.prevent="changeNote" v-if="edit">
                    <input type="text" placeholder="Title" required 
                        v-model="editNote.title">
                    <textarea required placeholder="Content" 
                        v-model="editNote.descr">
                    </textarea>
                    <div class="modal-btns">
                        <button href="#" class="btn" @click="changeNote">{{words.editwindowbtn[lang]}}</button>
                        <a href="#" class="btn" @click.prevent="$emit('closeModal')">{{words.closebtn[lang]}}</a>
                    </div>
                </form>
                <form @submit.prevent="send" v-else>
                    <input type="text" placeholder="Title" required v-model="user.title">
                    <textarea required placeholder="Content" v-model="user.descr"></textarea>
                    <div class="modal-btns">
                        <button href="#" class="btn">{{words.addbtn[lang]}}</button>
                        <a href="#" class="btn" @click.prevent="$emit('closeModal'), user.title = user.descr = ''">{{words.closebtn[lang]}}</a>
                    </div>
                </form>
            </div>
        </div>
    </Transition>
    <div class="container" v-else>
        <form class="modalWindow" required>
            <div class="input">
                <input type="text" required placeholder="Title"
                    v-if="edit"
                    v-model="editNote.title">
                    <input v-else type="text" placeholder="Title" required v-model="user.title">
                <span>Title</span>
            </div>
            <div class="textarea">
                <textarea rows="5" required placeholder="Content"
                    v-if="edit"
                    v-model="editNote.descr"></textarea>
                    <textarea v-else placeholder="Content" v-model="user.descr"></textarea>
                <span>Content</span>
            </div>
            <button href="#" class="btn" @click.prevent="changeNote" v-if="edit">{{words.editwindowbtn[lang]}}</button> 
            <button href="#" class="btn" @click.prevent="send" v-else>{{words.addbtn[lang]}}</button>
        </form>
    </div>
</template>

<script>
    export default {
        props: ['editNote', 'edit', 'lang', 'smallScreen'],
        inject: ['words'],
        data() {
            return {
                user: {
                    title: '',
                    descr: ''
                }
            }
        },
        methods: {
            send(){
                if(this.user.title.trim() != '' && this.user.descr.trim() != ''){
                    this.$emit('addNote', {...this.user})
                    this.$emit('closeModal')
                    for (const key in this.user) this.user[key] = ''
                }
            },
            changeNote(){
                if(this.editNote.title.trim() != '' && this.editNote.descr.trim() != ''){
                    const editedNote = {
                        id: this.editNote.id,
                        title: this.editNote.title,
                        descr: this.editNote.descr,
                        date: this.editNote.date.includes('edited') ? this.editNote.date : `edited ${this.editNote.date}`
                    }
                    this.$emit('editedNote', editedNote)
                    this.$emit('closeModal')
                }
            }
        }
    }
</script>

<style>

</style>