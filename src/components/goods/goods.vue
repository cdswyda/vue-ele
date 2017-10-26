<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul class="menu">
        <li class="menu-item" v-for="good in goods">
          <i v-show="good.type > 0"></i><span class="text">{{good.name}}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper"></div>
  </div>
</template>

<script type="text/ecmascript-6">
const ERR_OK = 0
export default {
  props: {
    seller: Object
  },
  data () {
    return {
      goods: []
    }
  },
  created () {
    this.$http.get('/api/goods').then(response => {
      const body = response.body
      console.log(response)
      if (body.errno === ERR_OK) {
        this.goods = body.data
      }
    })
  }
}
</script>


<style lang="stylus" scoped>
.goods {
  position: absolute;
  top: 174px;
  bottom: 46px;
  display: flex;
  width: 100%;
}

.menu-wrapper {
  flex: 0 0 80px;
  width: 80px;
  background: #f3f5f7;
}

.foods-wrapper {
  flex: 1;
}
</style>
