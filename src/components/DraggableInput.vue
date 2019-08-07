<template>
    <div class="wrapper">
     <!--   <div class="string-preview">
            {{value}}
        </div>-->
        <draggable v-model="wordsArray">
            <transition-group>
                <div class="word-item-wrapper tag is-light is-medium is-rounded"
                     v-for="(el,i) in wordsArray" :key="i">
                    <template v-if="!el.isInputShowing">
                        <span v-if="el.word" class="word-item" @click="wordClickHandler(el, i, ...arguments)">
                            {{el.word}}
                        </span>
                        <span class="icon ion-ios-close" @click="removeWordHandler(el, i, ...arguments)"></span>
                    </template>
                    <template v-else>
                        <input class="input is-small is-rounded" type="text" :key="i" v-model="el.word">
                        <span class="icon ion-ios-checkmark-circle-outline"
                              @click="wordClickHandler(el, i, ...arguments)">
                        </span>
                    </template>
                </div>
            </transition-group>
        </draggable>
    </div>
</template>

<script>
    import draggable from 'vuedraggable'

    export default {
        name: 'DraggableInput',
        components: {
            draggable
        },
        data() {
            return {
                wordsArray: []
            }
        },
        props: ['value'],
        watch: {
            wordsArray: function (val, oldVal) {
                this.combineString(val);
            }
        },
        created() {
            this.value && this.value.split(' ').forEach((word) => {
                if (!word) {
                    return;
                }
                this.wordsArray.push({word: word, isInputShowing: false});
            });
        },
        methods: {
            wordClickHandler: function (el, i, e) {
                el.isInputShowing = !el.isInputShowing;
                this.wordsArray = this.wordsArray.filter(el => el.word);
                this.combineString(this.wordsArray);
            },
            removeWordHandler: function (el, i, e) {
                this.wordsArray.splice(i, 1);
            },
            combineString: function (arr) {
                let new_string = arr.map(el => el.word).join(' ');
                this.$emit('input', new_string);
            }
        }
    }
</script>

<style scoped>
    .word-item {
        cursor: pointer;
    }

    .string-preview {
        margin: 5% 0;
    }

    .icon {
        cursor: pointer;
        transition: all 300ms;
    }

    .icon:hover {
        transform: scale(1.3);
    }

    .icon:active {
        transform: scale(1.1);
    }
</style>
