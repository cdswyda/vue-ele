<template>
  <header class="header">
    <!-- 内容区域 -->
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}} / {{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon icon-keyboard_arrow_right"></i>
      </div>
      <div class="background">
        <img :src="seller.avatar" width="100%" height="100%" alt="">
      </div>
    </div>
    <!-- 公告区域 -->
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon icon-keyboard_arrow_right"></i>
    </div>
    <!-- detail -->
    <transition name="panel-fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrap">
              <!-- star 组件 -->
              <star :size="48" :score="seller.score"></star>
            </div>

            <!-- 优惠信息 -->
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul class="supports" v-if="seller.supports">
              <li class="support-item" v-for="item in seller.supports">
                <span class="icon" :class="classMap[item.type]"></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>

            <!-- 公告 -->
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>

          </div>
        </div>
        <div class="detail-close">
          <i class="icon icon-close" @click="hideDetail"></i>
        </div>
      </div>
    </transition>
  </header>
</template>

<script type="text/ecmascript-6">
import Star from 'components/star/star.vue'

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail() {
      this.detailShow = true
    },
    hideDetail() {
      this.detailShow = false
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  },
  components: {
    star: Star
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl'

.header
  color: #fff
  background: rgba(7, 17, 27, 0.5)
  position: relative
  overflow: hidden

  .content-wrapper
    position: relative
    padding: 24px 12px 18px 24px
    font-size: 0

    .avatar
      display: inline-block
      vertical-align: top

      img
        border-radius: 2px

    .content
      font-size: 14px
      display: inline-block
      margin-left: 16px

      .title
        margin: 2px 0 8px 0

        .brand
          display: inline-block
          width: 30px
          height: 18px
          bg-image('./images/brand')
          background-size: 30px 18px
          background-repeat: no-repeat
          vertical-align: top

        .name
          margin-left: 6px
          font-size: 16px
          line-height: 18px
          font-weight: bold

      .description
        font-size: 12px
        margin-bottom: 10px
        line-height: 12px

      .support
        font-size: 0

        .icon
          display: inline-block
          vertical-align: top
          width: 12px
          height: 12px
          margin-right: 4px
          background-size: 12px 12px
          background-repeat: no-repeat

          &.decrease
            bg-image('./images/decrease_1')

          &.discount
            bg-image('./images/discount_1')

          &.guarantee
            bg-image('./images/guarantee_1')

          &.invoice
            bg-image('./images/invoice_1')

          &.special
            bg-image('./images/special_1')

        .text
          line-height: 12px
          font-size: 10px

    .support-count
      position: absolute
      right: 12px
      bottom: 14px
      padding: 0 8px
      height: 24px
      line-height: 24px
      border-radius: 14px
      background: rgba(0, 0, 0, 0.2)
      text-align: center

      .count
        font-size: 10px
        vertical-align: top
        line-height: 24px
        display: inline-block

      .icon
        margin-left: 2px
        font-size: 10px
        line-height: 24px

  .bulletin-wrapper
    position: relative
    height: 28px
    padding: 0 22px 0 12px
    line-height: 0
    font-size: 10px
    text-overflow: ellipsis
    white-space: nowrap
    overflow: hidden
    background: rgba(7, 17, 27, 0.2)

    .bulletin-title
      display: inline-block
      vertical-align: top
      margin-top: 8px
      width: 22px
      height: 12px
      bg-image('./images/bulletin')
      background-size: 22px 12px
      background-repeat: no-repeat

    .bulletin-text
      margin-left: 4px
      line-height: 28px

    .icon
      display: inline-block
      position: absolute
      right: 12px
      top: 9px

  .background
    position: absolute
    z-index: -1
    top: 0
    right: 0
    bottom: 0
    left: 0
    filter: blur(10px)

  .panel-fade-enter-active
    transition: all 0.3s ease

  .panel-fade-leave-active
    transition: all 0.4s cubic-bezier(1, 0.5, 0.8, 1)

  .panel-fade-enter, .panel-fade-leave-to
    background: rgba(7, 17, 27, 0.8)
    opacity: 0
    transform: rotateX(-90deg)
    transform-origin: 50% 0

  .detail
    position: fixed
    z-index: 100
    top: 0
    left: 0
    width: 100%
    height: 100%
    overflow: auto
    background: rgba(7, 17, 27, 0.8)
    backdrop-filter: blur(10px)
    transform-origin: 50% 0

    // transform: rotateX(0)
    // transition: all .4s
    // transform-origin: 50% 0
    .detail-wrapper
      width: 100%
      min-height: 100%
      overflow: auto

      .detail-main
        margin-top: 64px
        padding-bottom: 64px
        padding-left: 36px
        padding-right: 36px

        .name
          line-height: 16px
          font-size: 16px
          text-align: center

        .star-wrap
          margin-top: 18px
          text-align: center
          padding: 2px 0

        // 两边横线的小标题
        .title
          display: flex
          margin: 28px 0 0

          .line
            flex: 1
            position: relative
            top: -6px
            border-bottom: 1px solid rgba(255, 255, 255, 0.2)

          .text
            padding: 0 12px
            font-size: 14px
            font-weight: 700

        // 优惠信息内容
        .supports
          margin-top: 24px
          line-height: 16px
          font-size: 12px
          font-weight: 200

          .support-item
            margin-top: 12px

          .icon
            display: inline-block
            vertical-align: top
            width: 16px
            height: 16px
            margin-right: 4px
            background-size: 16px 16px
            background-repeat: no-repeat

            &.decrease
              bg-image('./images/decrease_2')

            &.discount
              bg-image('./images/discount_2')

            &.guarantee
              bg-image('./images/guarantee_2')

            &.invoice
              bg-image('./images/invoice_2')

            &.special
              bg-image('./images/special_2')

            .text
              margin-left: 6px

        // 公告
        .bulletin
          margin-top: 24px
          line-height: 24px
          font-size: 12px
          font-weight: 200

          .content
            text-indent: 2em

    .detail-close
      position: relative
      width: 32px
      height: 32px
      margin: -64px auto 0
      clear: both

      .icon
        font-size: 32px
</style>
