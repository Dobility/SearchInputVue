<template>
  <div id="app">
    <h1>搜索</h1>
    <SearchBox />
    <div class="link">
      <a href="">link</a>
      <a href="">link</a>
      <a href="">link</a>
    </div>
  </div>
</template>

<script>
import SearchBox from './components/SearchBox.vue'

export default {
  name: 'App',
  components: {
    SearchBox
  }
}
</script>

<style lang="less">
#app {
  margin: 60px;
  font-size: 15px;
  text-align: center;
}
.link {
  margin: 12px;
  a {
    margin: 12px;
  }
}
</style>
