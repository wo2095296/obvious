<template>
    <div>
        <div id="app">
            <img alt="Vue logo" src="assets/logo.png">
            <HelloWorld v-bind:text="text"/>
        </div>
    </div>
</template>

<script>
import HelloWorld from './HelloWorld.vue';

export default {
    name: 'app',
    components: {
        HelloWorld
    },
    data: function() {
        return {
            text:''
        }
    },
    methods:{
        changeText: function(text){
            this.text = text;
        }
    },
    created: function() {
        const vueSocket = window.globalBus.getSocket('vueSocket');
        this.text = vueSocket.getState('text');
        vueSocket.watchState('text', this.changeText.bind(this));
    },
    beforeDestroyed: function() {
        console.log('before destroy');
        const vueSocket = window.globalBus.getSocket('vueSocket');
        vueSocket.unwatch('text', this.changeText);
    }
}
</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
