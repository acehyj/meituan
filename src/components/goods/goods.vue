<template>
    <div class="goods">
        <div class="scroll-nav-wrapper">
            <cube-scroll-nav :side="true" 
                             :data="goods" 
                             :options="scrollOptions" 
                             v-if="goods.length"
            >
                <template v-slot:bar="props">
                    <cube-scroll-nav-bar direction="vertical" 
                                         :labels="props.labels" 
                                         :txts="barTxts" 
                                         :current="props.current">
                        <!-- 如果:txts里面的内容不是默认的，那么就使用插槽自己定义展示的内容 -->
                        <template v-slot="props">
                            <div class="text">
                                 <img class="icon" 
                                      v-if="props.txt.type>=0" 
                                      :src="props.txt.type===2?hotIcon:discountIcon"
                                 ></img>
                                <span>{{props.txt.name}}</span>
                                <span class="num" v-if="props.txt.count">
                                    <bubble :num="props.txt.count"></bubble>
                                </span>
                            </div>
                        </template>
                    </cube-scroll-nav-bar>
                </template>
                <cube-scroll-nav-panel v-for="good in goods" :key="good.name" :label="good.name" :title="good.name">
                    <ul>
                        <li v-for="food in good.foods" :key="food.name" class="food-item" @click="selectFood(food)">
                            <div class="icon">
                                <img  :src="food.icon">
                            </div>
                            <div class="content">
                                <h2 class="name">{{food.name}}</h2>
                                <p class="desc">{{food.description}}</p>
                                <div class="extra">
                                    <span class="count">月售{{food.sellCount}}份</span>
                                    <span>赞{{food.praiseNum}}</span>
                                </div>
                                <div class="price">
                                    <span class="now">￥{{food.price}}</span>
                                    <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                                </div>
                                <div class="cart-control-wrapper">
                                    <cart-control :food="food"></cart-control>
                                </div>
                            </div>
                        </li>
                    </ul>
                </cube-scroll-nav-panel>
            </cube-scroll-nav>
        </div>
        <div class="shop-cart-wrapper">
            <shop-cart ref="shopcat" 
                       :select-foods="selectFoods" 
                       :delivery-price="seller.deliveryPrice" 
                       :min-price="seller.minPrice"
            ></shop-cart>
        </div>
    </div>
</template>
<script>
    import { getGoods } from 'api'
    import ShopCart from 'components/shop-cart/shop-cart'
    import CartControl from 'components/cart-control/cart-control'
    import Bubble from 'components/bubble/bubble'
    export default {
        name: 'goods',
        props: {
            data: {
                type: Object,
                default() {
                    return {}
                }
            }
        },
        data() {
            return {
                goods: [],
                selectedFood: {},
                scrollOptions: {
                    click: false,
                    directionLockThreshold: 0
                },
                hotIcon: "https://p1.meituan.net/aichequan/87f966955f693102d67daf2ec44b58411361.png",
                discountIcon: "https://p0.meituan.net/aichequan/45662b4d1968fd75bff38de23f6d62641421.png"
            }
        },
        computed: {
            seller() {
                return this.data.seller
            },
            selectFoods() {
                let ret = []
                this.goods.forEach((good) => {
                    good.foods.forEach((food) => {
                        if (food.count) {
                            ret.push(food)
                        }
                    })
                })
                return ret
            },
            barTxts() {
                let ret = []
                this.goods.forEach((good) => {
                    const { type, name, foods } = good
                    let count = 0
                    foods.forEach((food) => {
                        // food默认是没有count属性的
                        count += food.count || 0
                    }) 
                    ret.push({
                        type,
                        name,
                        count
                    })
                })
                return ret
            }
        },
        methods: {
            fetch() {
                getGoods().then((goods) => {
                    this.goods = goods
                })
            },
            selectFood(food) {
                this.selectedFood = food
                this._showFood()
            },
            _showFood() {
                this.foodComp = this.foodComp || this.$createFood({
                    $props: {
                        food: 'selectedFood'
                    }
                })
                this.foodComp.show()
            }
        },
        components: {
            ShopCart,
            CartControl,
            Bubble
        }
    }
</script>
<style lang="stylus" scoped>
@import '~common/stylus/variable'

.goods 
    position: relative
    height: 100%

    .scroll-nav-wrapper 
        position: absolute
        width: 100%
        top: 0
        left: 0
        bottom: 48px
  
    >>> .cube-scroll-nav-bar 
        width: 80px
        white-space: normal
        overflow: hidden
        background: $color-background-ssss

    >>> .cube-scroll-wrapper 
        width: 100%

    >>> .cube-scroll-nav-bar-item
        padding: 0 10px
        display: flex
        align-items: center
        height: 56px
        font-size: $fontsize-small

        .text 
            flex: 1
            position: relative
            .icon 
                width: 15px
                height: 15px
                margin-right: 4px
                vertical-align: bottom
            .num 
                position: absolute
                right: -8px
                top: -10px

    >>> .cube-scroll-nav-bar-item_active 
        background: $color-white
        color: $color-dark-grey

    >>> .cube-scroll-nav-panel-title 
        padding-left: 14px
        height: 26px
        line-height: 26px
        border-left: 2px solid $color-col-line
        font-size: $fontsize-small
        color: $color-grey
        background: $color-background-ssss

    .food-item 
        display: flex
        margin: 10px 10px 20px 10px
        position: relative

        .icon 
            flex: 0 0 75px
            margin-right: 10px
            img 
                width: 75px
                height: 75px

        .content 
            flex: 1

            .name 
                margin: 2px 0 8px 0
                height: 14px
                line-height: 14px
                font-size: $fontsize-medium
                color: $color-dark-grey

            .desc, .extra 
                line-height: 12px
                font-size: $fontsize-small-s
                color: $color-light-grey

            .desc 
                margin-bottom: 8px
                width: 200px
                overflow: hidden
                text-overflow: ellipsis
                white-space: nowrap
            
            .extra 
                .count 
                    margin-right: 12px

            .price 
                font-weight: 700
                line-height: 24px

                .now 
                    margin-right: 8px
                    font-size: $fontsize-medium
                    color: $color-red

                .old 
                    text-decoration: line-through
                    font-size: $fontsize-small-s
                    color: $color-light-grey

        .cart-control-wrapper 
            position: absolute
            right: 0
            bottom: 0

    .shop-cart-wrapper 
        position: absolute
        left: 0
        bottom: 0
        z-index: 50
        width: 100%
        height: 48px

</style>
