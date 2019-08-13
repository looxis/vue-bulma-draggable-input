<template>
    <div class="wrapper">
        <!--   <div class="string-preview">
               {{value}}
           </div>-->
        <draggable v-model="wordsArray" v-bind="dragOptions" class="list-group" tag="ul" @start="drag = true"
                   @end="drag = false">
            <transition-group type="transition" :name="!drag ? 'flip-list' : null">
                <div class="word-item-wrapper tag is-light is-medium is-rounded"
                     v-for="(el,i) in wordsArray" :key="i">
                    <template v-if="!el.isInputShowing">
                        <span class="word-item" @click="wordClickHandler(el, i, ...arguments)">
                            {{el.word}}
                        </span>
                        <button class="delete is-small" @click="removeWordHandler(el, i, ...arguments)"></button>
                    </template>
                    <template v-else>
                        <input class="word-input input is-small is-rounded" type="text" :key="i" v-model="el.word"
                               autofocus>
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
    import '../../node_modules/bulma/css/bulma.css';

    export default {
        name: 'DraggableInput',
        components: {
            draggable
        },
        data() {
            return {
                wordsArray: [],
                drag: false
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
        },
        computed: {
            dragOptions() {
                return {
                    animation: 100,
                    group: "description",
                    disabled: false,
                    ghostClass: "ghost"
                };
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

    .button {
        margin-top: 35px;
    }

    .flip-list-move {
        transition: transform 0.5s;
    }

    .no-move {
        transition: transform 0s;
    }

    .ghost {
        opacity: 0.5;
        background: #c8ebfb;
    }

    .list-group {
        min-height: 20px;
    }

    .list-group-item {
        cursor: move;
    }

    .list-group-item i {
        cursor: pointer;
    }
</style>
