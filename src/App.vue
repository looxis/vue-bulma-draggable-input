<template>
    <div id="app">
        <draggable-input v-model="input_data"/>
    </div>
</template>

<script>
    import DraggableInput from './components/DraggableInput.vue'

    export default {
        name: 'app',
        components: {
            DraggableInput
        },
        data() {
            let data = {
                input_data: "Welcome to Your Vue.js App"
            };
            return data;
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
