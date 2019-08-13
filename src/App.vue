<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <div>
      <h2> 图片展示排序 </h2>
    </div>
    <PicBox />
  </div>
</template>

<script>
import PicBox from './components/PicBox.vue'

export default {
  name: 'app',
  components: {
    PicBox
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
