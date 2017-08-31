<template>
<div id="app">
  <v-header :seller="seller"></v-header>
  <nav class="nav">
    <div class="nav-item">商品</div>
    <div class="nav-item">评论</div>
    <div class="nav-item">商家</div>
  </nav>
  <div class="content"></div>
  <img src="./assets/logo.png">
  <router-view></router-view>
</div>
</template>

<script>
import Header from './components/header/header'

export default {
  name: 'app',
  data () {
    return {
      seller: {}
    }
  },
  created () {
    // let me = this
    this.$http.get('/api/seller').then((response) => {
      response = response.body
      if (response.errno === 0) {
        this.seller = response.data
      }
    })
  },
  components: {
    'v-header': Header
  }
}
</script>

<style lang="stylus">
#app {
  // font-family: 'Avenir', Helvetica, Arial, sans-serif;
  // -webkit-font-smoothing: antialiased;
  // -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  // color: #2c3e50;
  // margin-top: 60px;
}
.nav
  display: flex
  width: 100%
  height: 40px
  line-height: 40px
  text-align: center
  .nav-item
    flex: 1
</style>
