<template>
    <header class="header">
        <Transition name="header-anim" mode="out-in">
            <div class="header-notes" v-if="(header && !showModal) || (header && showModal && !smallScreen)">
                <button @click="changeLang" v-if="lang == 'uz'">UZ</button>
                <button @click="changeLang" v-else>RU</button>
                <h1>{{words.appbartitle[lang]}}</h1>
                <button @click="header = false"><img src="@/assets/img/search.svg" alt=""></button>
            </div> 
            <div class="header-form" v-else-if="!header && !showModal">
                <img src="@/assets/img/back.svg" alt="" @click="header = true, search = ''">
                <input type="text" :placeholder="words.appbarseacrch[lang]" v-model="search" required>
                <button @click="search = ''"><img src="@/assets/img/cancel.svg" alt=""></button>
            </div>
            <div class="header-modal" v-else-if="showModal && smallScreen">
                <img src="@/assets/img/back.svg" alt="" @click="showModal = false, $emit('closeModal')">
                <h1>{{edit ? words.titlewindowedit[lang] : words.titlewindow[lang]}}</h1>
            </div>
        </Transition>
    </header>
</template> 

<script>
    export default {
        props: ['lang', 'showModal', 'smallScreen', 'edit'],
        inject: ['words'],
        data(){
            return {
                search: '',
                header: true,
            }
        },
        watch: {
            search(val){
                this.$emit('searchValue', val)
            }
        },
        methods: {
            changeLang(){
                this.$emit('changeLang', this.lang == 'ru' ? 'uz' : 'ru')
            },
        }
    }
</script>

<style>

</style>
