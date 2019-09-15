<template>
    <div v-show="visible" class="food" @click="click">
        <transition name="pop">
            <div class="food-wrapper">
                <div class="food-detail">
                    <img :src="food.image" class="image" alt="">
                    <div class="content">
                        <h2 class="name">{{food.name}}</h2>
                        <div class="extra">
                            <span class="count">月售{{food.sellCount}}份</span><span>赞{{food.praiseNum}}</span>
                        </div>
                        <p class="desc">{{food.description}}</p>
                    </div>
                    <div class="bottom">
                        <span class="price">￥{{food.price}}</span>
                        <div class="add-wrapper" @click="addFirst" v-if="!food.count">
                            <span class="add">加入购物车</span>
                        </div>
                        <div class="cart-control-wrapper" v-if="food.count">
                            <cart-control :food="food"></cart-control>
                        </div>
                    </div>
                </div>
                <div class="close">
                    <div class="icon">×</div>
                </div>
            </div>
        </transition>
    </div>
</template>
<script>
    import CartControl from 'components/cart-control/cart-control'
    import popupMixin from 'common/mixins/popup'

    export default {
        name: 'food',
        mixins: [popupMixin],
        props: {
            food: {
                type: Object
            }
        }, 
        components: {
            CartControl
        },
         methods: {
            addFirst(event) {
                this.$set(this.food, 'count', 1)
            },
            click() {
                this.hide()
            }
        }
    }
</script>
<style lang="stylus" scoped>
@import '~common/stylus/variable'
.food 
    position: fixed
    z-index: 100
    top: 0
    left: 0
    width: 100%
    height: 100%
    overflow: auto
    backdrop-filter: blur(10px)
    opacity: 1
    color: $color-gray
    background: $color-background-ss
    .food-wrapper
        animation: mymove 0.1s linear 
        @keyframes mymove
        {
            0%{transform: scale(0.5)}
            100%{transform: scale(1)}

        }
        position: absolute;
        width: 75%
        height: 65%
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: auto
        .food-detail
            position: absolute;
            width: 100%
            height: 85%
            background: $color-white
            border-radius: 10px 
            overflow: hidden
            .image
                width: 100%
                height: 60%
            .content
                padding: 8px 16px
                .name
                    font-size: $fontsize-large-x
                    font-weight: bold
                    padding-bottom: 6px
                .extra
                    font-size: $fontsize-small
                    padding-bottom: 8px
                    color: $color-grey
                .desc
                    font-size: $fontsize-small
                    color: $color-grey
            .bottom
                position: absolute
                bottom: 0
                width: 100%
                height: 10%
                box-sizing: border-box
                padding: 0 16px 10px 16px
                display: flex
                justify-content: space-between
                align-items: center
                .price
                    font-size: $fontsize-large-xxx
                    color: $color-red 
                    font-weight: bold
                .add-wrapper
                    display: inline-block
                    float: right
                    padding: 8px 10px                    
                    background: $color-yellow
                    border-radius: 20px
                .add
                    font-size: $fontsize-small
                    color: $color-gray
        .close
            position: absolute
            bottom: 0
            width: 40px
            height: 40px
            left: 50%
            margin-left: -20px
            border-radius: 50%
            background: $color-background-sss
            .icon
                font-size: $fontsize-large-xxxx
                color: $color-white
                text-align: center
                height: 40px
                line-height: 40px
                margin: 1px 4px

</style>