<template>
  <transition name="move">
    <div v-show="showFlag" class="food" ref="food">
      <div>
        <div class="back" @click="hide">
          <i class="fa fa-angle-left"></i>
        </div>
        <div class="food-image">
          <img :src="food.image" alt="">
        </div>
        <div class="detail">
          <h2 class="name">{{food.name}}</h2>
          <div class="extra">
            <span class="count">月售{{food.sellCount}}份</span>
            <span>好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="cur-price">&yen;{{food.price}}</span>
            <span v-show="food.oldPrice" class="old-price">&yen;{{food.oldPrice}}</span>
          </div>
          <div v-show="!food.count" class="buy" @click="addFirst">加入购物车</div>
          <div v-show="food.count" class="cartcontrol-wrap">
            <cartcontrol :food="food"></cartcontrol>
          </div>
        </div>
        <split v-show="food.info"></split>
        <div v-show="food.info" class="info">
          <h2 class="title">商品介绍</h2>
          <p class="text">{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
          <h2 class="title">商品评价</h2>
          <ratingselect
            @select="selectRating"
            @toggle="toggleContent"
            :ratings="food.ratings"
            :select-type="selectType"
            :only-content="onlyContent" :desc="desc"></ratingselect>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'
  import BScroll from 'better-scroll'
  import cartcontrol from '@/components/cartcontrol/cartcontrol'
  import split from '@/components/split/split'
  import ratingselect from '@/components/ratingselect/ratingselect'

  const ALL = 2

  export default {
    props: {
      food: {
        type: Object
      }
    },
    data () {
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          positive: '推荐',
          negative: '吐槽',
          all: '全部'
        }
      }
    },
    methods: {
      show () {
        this.showFlag = true
        this.selectType = ALL
        this.onlyContent = true
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            })
          } else {
            this.scroll.refresh()
          }
        })
      },
      hide () {
        this.showFlag = false
      },
      addFirst () {
        if (!event._constructed) {
          return
        }
        Vue.set(this.food, 'count', 1)
      },
      toggleContent () {
        this.onlyContent = !this.onlyContent
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      },
      selectRating (type) {
        this.selectType = type
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      }
    },
    components: {
      cartcontrol,
      split,
      ratingselect
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  .food {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 48px;
    background-color: #fff;
    transform: translate3D(0, 0, 0);
    transition: all .5s;
    .back {
      position: absolute;
      top: 10px;
      left: 10px;
      z-index: 99;
      width: 35px;
      height: 35px;
      font-size: 24px;
      line-height: 33px;
      color: #fff;
      text-align: center;
      border-radius: 50%;
      background-color: rgba(7, 17, 27, .2);
    }
    &-image {
      position: relative;
      width: 100%;
      height: 0;
      padding-top: 100%;
      img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
      }
    }
    .detail {
      position: relative;
      padding: 18px;
      .name {
        margin-bottom: 8px;
        font-size: 14px;
        font-weight: 700;
        color: rgb(7, 17, 27);
      }
      .extra {
        margin-bottom: 8px;
        font-size: 10px;
        color: rgb(147, 153, 159);
      }
      .count {
        margin-right: 12px;
      }
      .price {
        line-height: 24px;
      }
      .cur-price {
        margin-right: 12px;
        font-size: 14px;
        font-weight: 700;
        color: rgb(240, 20, 20);
      }
      .old-price {
        font-size: 10px;
        color: rgb(147, 153, 159);
      }
      .buy {
        position: absolute;
        right: 18px;
        bottom: 18px;
        padding: 0 12px;
        line-height: 24px;
        text-align: center;
        font-weight: 400;
        color: #fff;
        border-radius: 12px;
        background-color: rgb(0, 160, 220);
      }
      .cartcontrol-wrap {
        position: absolute;
        right: 12px;
        bottom: 11px;
      }
    }
    .info {
      padding: 18px;
      .title {
        margin-bottom: 8px;
        font-size: 14px;
        font-weight: 400;
        color: rgb(7, 17, 27);
      }
      .text {
        font-size: 12px;
        line-height: 24px;
        text-align: justify;
        color: rgb(77, 85, 93);
      }
    }
    .rating {
      .title {
        padding: 18px;
        padding-bottom: 0;
        font-size: 14px;
        font-weight: 400;
        color: rgb(7, 17, 27);
      }
    }
  }

  .move {
    &-enter-active,
    &-leave-active {
      transform: translate3D(0, 0, 0);
    }
    &-enter,
    &-leave-active {
      transform: translate3D(100%, 0, 0);
    }
  }
</style>