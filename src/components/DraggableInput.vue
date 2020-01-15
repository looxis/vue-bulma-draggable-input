<template>
    <div class="wrapper">
        <!--   <div class="string-preview">
               {{value}}
           </div>-->
        <draggable v-model="wordsArray" v-bind="dragOptions" class="list-group" tag="ul" :style="styles.wordItem">
            <transition-group type="transition">
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
                        <span class="icon" @click="wordClickHandler(el, i, ...arguments)">
                            <svg aria-hidden="true" :style="styles.checkIcon" focusable="false" data-prefix="far"
                                 data-icon="check-circle"
                                 role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"
                                 class="svg-inline--fa fa-check-circle fa-w-16 fa-2x">
                                <path fill="currentColor"
                                      d="M256 8C119.033 8 8 119.033 8 256s111.033 248 248 248 248-111.033 248-248S392.967 8 256 8zm0 48c110.532 0 200 89.451 200 200 0 110.532-89.451 200-200 200-110.532 0-200-89.451-200-200 0-110.532 89.451-200 200-200m140.204 130.267l-22.536-22.718c-4.667-4.705-12.265-4.736-16.97-.068L215.346 303.697l-59.792-60.277c-4.667-4.705-12.265-4.736-16.97-.069l-22.719 22.536c-4.705 4.667-4.736 12.265-.068 16.971l90.781 91.516c4.667 4.705 12.265 4.736 16.97.068l172.589-171.204c4.704-4.668 4.734-12.266.067-16.971z"
                                      class=""></path>
                            </svg>
                        </span>
                        <!--          <span class="icon ion-ios-checkmark-circle-outline"
                                        @click="wordClickHandler(el, i, ...arguments)">
                                  </span>-->
                    </template>
                </div>
            </transition-group>
        </draggable>
    </div>
</template>

<script>
    import draggable from 'vuedraggable'

    const styles = {
        wordItem: {
            'cursor': 'pointer'
        },
        checkIcon: {
            'width': '18px'
        }
    };

    export default {
        name: 'DraggableInput',
        components: {
            draggable
        },
        data() {
            return {
                wordsArray: [],
                styles: styles
            }
        },
        props: ['value'],
        watch: {
            wordsArray: function (val) {
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
            wordClickHandler: function (el) {
                el.isInputShowing = !el.isInputShowing;
                this.wordsArray = this.wordsArray.filter(el => el.word);
                this.combineString(this.wordsArray);
            },
            removeWordHandler: function (el, i) {
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
