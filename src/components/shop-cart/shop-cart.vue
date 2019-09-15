<template>
    <div>
        <div class="shopcart">
            <div class="content" @click="toggleList">
                <div class="content-left">
                    <div class="logo-wrapper">
                        <div class="logo" :class="{'highlight':totalCount>0}">
                            <i class="iconfont icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
                        </div>
                        <div class="num" v-if="totalCount>0">
                            <bubble :num="totalCount"></bubble>
                        </div>
                    </div>
                    <div class="text">
                        <div class="price" v-if="totalPrice>0" :class="{'highlight':totalPrice>0}">
                            <span class="yuan">￥</span>{{totalPrice}}
                        </div>
                        <div class="desc" :class="{'empty':totalPrice===0}">
                            另需配送费￥{{deliveryPrice}}
                        </div>
                    </div>
                </div>
                <div class="content-right" @click="pay">
                    <div class="pay" :class="payClass">
                        {{payDesc}}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
  import Bubble from 'components/bubble/bubble'

  export default {
    name: 'shop-cart',
    props: {
      selectFoods: {
        type: Array,
        default() {
          return []
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      },
      sticky: {
        type: Boolean,
        default: false
      },
      fold: {
        type: Boolean,
        default: true
      }
    },
    data() {
      return {
        listFold: this.fold
      }
    },
    computed: {
      totalPrice() {
        let total = 0
        this.selectFoods.forEach((food) => {
          total += food.price * food.count
        })
        return total
      },
      totalCount() {
        let count = 0
        this.selectFoods.forEach((food) => {
          count += food.count
        })
        return count
      },
      payDesc() {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice
          return `还差￥${diff}元起送`
        } else {
          return '去结算'
        }
      },
      payClass() {
        if (!this.totalCount || this.totalPrice < this.minPrice) {
          return 'not-enough'
        } else {
          return 'enough'
        }
      }
    },
    methods: {
        pay(e) {
            if (this.totalPrice < this.minPrice) {
              return
            }
            this.$createDialog({
              title: '支付',
              content: `您需要支付${this.totalPrice}元`
            }).show()
            e.stopPropagation()
        },
        toggleList() {
            if (this.listFold) {
                if (!this.totalCount) {
                    return
                }
                  this.listFold = false
                  this._showShopCartList()
                  this._showShopCartSticky()
                } else {
                  this.listFold = true
                  this._hideShopCartList()
            }
        },
        
         _showShopCartList() {
            this.shopCartListComp = this.shopCartListComp || this.$createShopCartList({
              $props: {
                selectFoods: 'selectFoods'
              },
              $events: {
                leave: () => {
                  this._hideShopCartSticky()
                },
                hide: () => {
                  this.listFold = true
                }
              }
            })
            this.shopCartListComp.show()
        },
      _showShopCartSticky() {
        this.shopCartStickyComp = this.shopCartStickyComp || this.$createShopCartSticky({
          $props: {
            selectFoods: 'selectFoods',
            deliveryPrice: 'deliveryPrice',
            minPrice: 'minPrice',
            fold: 'listFold',
            list: this.shopCartListComp
          }
        })
        this.shopCartStickyComp.show()
      },
      _hideShopCartList() {
        const list = this.sticky ? this.$parent.list : this.shopCartListComp
        list.hide && list.hide()
      },
      _hideShopCartSticky() {
        this.shopCartStickyComp.hide()
      }
    },
    watch: {
      fold(newVal) {
        this.listFold = newVal
      },
      totalCount(count) {
        if (!this.listFold && count === 0) {
          this._hideShopCartList()
        }
      }
    },
    components: {
      Bubble
    }
  }
</script>

<style lang="stylus" scoped>
@import '~common/stylus/variable'

.shopcart 
    height: 100%
    .content 
        display: flex
        background: $color-background-grey
        color: $color-light-grey
        .content-left 
            flex: 1
            display: flex
            .logo-wrapper 
                display: inline-block
                vertical-align: top
                position: relative
                top: -10px
                margin: 0 12px
                width: 50px
                height: 50px
                box-sizing: border-box
                border-radius: 50%
                background: $color-background-grey
                .logo 
                    width: 100%
                    height: 100%
                    border-radius: 50%
                    text-align: center
                    background: $color-dark-grey
                    &.highlight 
                        background: $color-yellow
                    .icon-shopping_cart 
                        line-height: 50px
                        font-size: $fontsize-large-xxx
                        color: $color-light-grey
                        &.highlight 
                            color: $color-background-grey
                .num 
                    position: absolute
                    top: 0
                    right: 0
                    font-size: $fontsize-small-s
            .text
                display: flex
                flex-direction: column
                .price 
                    font-weight: 600
                    margin: 3px 0
                    font-size: $fontsize-large-xx
                    &.highlight 
                        color: $color-white
                    .yuan
                        font-size: $fontsize-small

                .desc 
                    margin: 2px 0
                    font-size: $fontsize-small-s
                    &.empty
                        font-size: $fontsize-medium
                        line-height: 48px

        .content-right 
            flex: 0 0 105px
            width: 105px
            .pay 
                height: 48px
                line-height: 48px
                text-align: center
                font-weight: 700
                font-size: $fontsize-medium
                &.not-enough 
                    background: $color-dark-grey
                &.enough 
                    background: $color-yellow
                    color: $color-dark-grey

</style>