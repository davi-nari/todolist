<template>
    <Transition name="modal">
        <div class="modal" @click="$emit('closeModal')">
            <div class="modal-content" @click.stop>
                <h2>{{words.deledit[lang]}}</h2>
                <span v-if="lang == 'ru'">{{words.titlewindowdel[lang]}} <span class="modal-content-title">{{delTitle}}?</span></span>
                <span v-else><span class="modal-content-title">{{delTitle}}</span> {{words.titlewindowdel[lang]}}?</span>
                <div class="modal-btns-del">
                    <a href="#" class="btn" @click.prevent="$emit('closeModal')">{{words.closebtn[lang]}}</a>
                    <a href="#" class="btn" @click.prevent="$emit('deleteNote')">{{words.deledit[lang]}}</a>
                </div>
            </div>
        </div>    
    </Transition>
</template>

<script>
    export default {
        props:['lang', 'delTitle'],
        inject: ['words'],
        data() {
            return {
                
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>