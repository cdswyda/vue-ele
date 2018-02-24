<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menu">
      <ul class="menu">
        <li class="menu-item" v-for="(good,index) in goods" :key="index" :class="{'active':currentIndex === index}" @click="selectMenu(index,$event)">
          <span class="text"><i v-show="good.type > 0" class="icon" :class="classMap[good.type]"></i>{{good.name}}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foods">
      <ul>
        <li v-for="(item,index) in goods" :key="index" class="food-cate-item food-cate-item-hook">
          <h1 class="food-list-title">{{item.name}}</h1>
          <ul class="food-list">
            <li v-for="(food,index) in item.foods" :key="index" class="food-item">
              <div class="food-icon">
                <img :src="food.icon" alt="">
              </div>
              <div class="food-content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span>月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥<span class="num">{{food.price}}</span></span>
                  <span v-if="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll'
const ERR_OK = 0
export default {
  props: {
    seller: Object
  },
  data() {
    return {
      goods: [],
      listHeights: [],
      scrollY: 0
    }
  },

  created() {
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']

    this.$http.get('/api/goods').then(response => {
      const body = response.body
      console.log(response)
      if (body.errno === ERR_OK) {
        this.goods = body.data
        // this.initScroll()
        this.$nextTick(() => {
          this.initScroll()
          this.calcuateHeight()
        })
        // setTimeout(() => {
        //   this.initScroll()
        // }, 1200)
      }
    })
  },
  computed: {
    currentIndex() {
      for (let i = 0; i < this.listHeights.length; i++) {
        let h1 = this.listHeights[i]
        let h2 = this.listHeights[i + 1]
        if (!h2 || (this.scrollY >= h1 && this.scrollY < h2)) {
          return i
        }
      }
      return 0
    }
  },
  methods: {
    initScroll() {
      this.menuScroll = new BScroll(this.$refs.menu, {
        click: true
      })
      this.foodsScroll = new BScroll(this.$refs.foods, {
        probeType: 3
      })

      this.foodsScroll.on('scroll', pos => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    calcuateHeight() {
      const foodsList = this.$refs.foods.getElementsByClassName(
        'food-cate-item-hook'
      )
      let h = 0
      this.listHeights.push(h)
      for (let i = 0, l = foodsList.length; i < l; ++i) {
        h += foodsList[i].clientHeight
        this.listHeights.push(h)
      }
    },
    selectMenu(index, e) {
      if (!e._constructed) return
      console.log(index, e)
      this.foodsScroll.scrollTo(0, -this.listHeights[index], 300)
    }
  }
}
</script>


<style lang="stylus" scoped>
@import '../../common/stylus/mixin.styl';

.goods {
  position: absolute;
  top: 174px;
  bottom: 46px;
  display: flex;
  width: 100%;
  overflow: hidden;
}

.menu-wrapper {
  flex: 0 0 80px;
  width: 80px;
  background: #f3f5f7;
}

.menu-item {
  display: table;
  width: 54px;
  height: 56px;
  font-size: 12px;
  line-height: 14px;
  padding: 0 12px;

  &.active {
    background: #fff;
    font-weight: 800;
  }

  .icon {
    display: inline-block;
    vertical-align: top;
    width: 12px;
    height: 12px;
    margin-right: 2px;
    background-size: 12px 12px;
    background-repeat: no-repeat;

    &.decrease {
      bg-image('./images/decrease_3');
    }

    &.discount {
      bg-image('./images/discount_3');
    }

    &.guarantee {
      bg-image('./images/guarantee_3');
    }

    &.invoice {
      bg-image('./images/invoice_3');
    }

    &.special {
      bg-image('./images/special_3');
    }
  }

  .text {
    font-size: 12px;
    display: table-cell;
    width: 56px;
    border-1px(rgba(7, 17, 27, 0.1));
    vertical-align: middle;
  }
}

.foods-wrapper {
  flex: 1;
}

.food-list-title {
  height: 26px;
  font-size: 12px;
  color: rgb(147, 153, 159);
  line-height: 26px;
  background: #f3f5f7;
  padding-left: 14px;
  position: relative;

  &:before {
    content: '';
    display: block;
    height: 100%;
    width: 0;
    position: absolute;
    left: 0;
    top: 0;
    border-right: 1px solid #d9dde1;
  }
}

.food-list {
  background: #fff;
}

.food-item {
  display: flex;
  margin: 18px;
  padding-bottom: 18px;
  border-1px(rgba(7, 17, 27, 0.1));

  &:last-child {
    margin-bottom: 0;
    border-none();
  }

  .food-icon {
    flex: 0 0 57px;
    margin-right: 10px;
  }

  .food-content {
    flex: 1;

    .name {
      font-size: 14px;
      line-height: 14px;
      padding: 2px 0 8px;
      height: 14px;
      color: rgb(7, 17, 27);
    }

    .desc, .extra {
      font-size: 10px;
      line-height: 10px;
      color: rgb(147, 153, 159);
    }

    .desc {
      margin-bottom: 8px;
    }

    .extra {
      &>span+span {
        margin-left: 12px;
      }
    }

    .price {
      color: rgb(147, 153, 159);
      font-weight: 700;
      line-height: 24px;
      font-size: 10px;

      .now {
        &>.num {
          font-size: 14px;
        }

        color: rgb(240, 20, 20);
      }

      .old {
        text-decoration: line-through;
      }
    }
  }
}
</style>
