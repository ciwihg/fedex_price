<template>
  <div id="app">
    <HelloWorld />
  </div>
</template>

<script>
import Particle from "./components/background.js"
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  mounted(){
    new Particle(document.getElementById('app'),{
    dotColor: '#b39ddb',
    lineColor: '#b388ff'
});
  }
}
</script>

<style>
html{
    height: 100%;
}
body{
  height: 100%;
}
#app {
  font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;

}
</style>
