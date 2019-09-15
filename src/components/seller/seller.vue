<template>
    <cube-scroll class="seller" :options="sellerScrollOptions">
        <div class="seller-wrapper">
            <div class="seller-info">
                <div class="address-wrapper">
                    <span class="iconfont icon-address"></span>
                    <span class="address">{{seller.address}}</span>
                </div>
                 <div class="qualification">
                    <span class="iconfont icon-qualification"></span>
                    <span>查看食品安全档案</span>
                    <span class="iconfont icon-keyboard_arrow_right"></span>
                </div>
            </div>
            <div class="delivery">
                <div class="deliveryServ">
                    <span class="iconfont icon-time"></span>
                    <span class="content">配送服务：提供高品质配送服务</span>
                </div>
                <div class="time">
                    <span class="iconfont icon-time"></span>
                    <span class="content">配送时间：{{seller.shipping_time}}</span>
                </div>
            </div>
            <div class="service" v-if="seller.service">
                <span class="iconfont icon-service"></span>
                <span class="title">商家服务</span>
                <div class="content">
                    <div class="item" v-for="item in seller.service" :key="item.content">
                        <img :src="item.icon" alt="" class="image">
                        <span>{{item.content}}</span>
                    </div>
                </div>
            </div>
            <div class="support" v-if="seller.support">
                <div class="item" v-for="item in seller.support" :key="item.type">
                    <img :src="item.icon" alt="" class="icon">
                    <span>{{item.description}}</span>
                </div>
            </div>
        </div>
    </cube-scroll>
</template>
<script>
    export default {
        name: 'seller',
        props: {
            data: {
                type: Object,
                default() {
                    return {}
                }
            }
        },
        computed: {
            seller() {
                return this.data.seller || {}
            }
        },
        data() {
            return {
                sellerScrollOptions: {
                    directionLockThreshold: 0,
                    click: false
                },

            }
        }
}
</script>
<style lang="stylus" scoped>
@import '~common/stylus/variable'

.seller 
    height: 100%;
    text-align: left;
    color: $color-dark-grey
    font-size: $fontsize-medium
    font-family: mtsi-font
    padding:  0 15px
    .iconfont
        color: $color-grey
        font-size: $fontsize-large-xx
        padding-right: 8px
        vertical-align:text-top
    .seller-info
        display: flex
        flex-direction: column
        justify-content: center
        border-bottom:  1px solid $color-row-line
        .address-wrapper
            display: flex
            overflow: hidden
            white-space: normal
            margin: 18px 0
            .address
                line-height: 20px
        .qualification
            height: 14x
            line-height: 14px
            margin-bottom: 18px 
            .icon-keyboard_arrow_right
                float: right 
    .delivery
        border-bottom: 1px solid $color-row-line
        .deliveryServ
             margin: 18px 0
        .time
            margin-bottom: 18px                   
    .service
        display:flex
        margin: 18px 0
        .title
            padding-top: 3px
        .content
            display:flex
            flex-direction: column
            justify-content: center
            padding-left: 10px
            .item
                margin-bottom: 5px 
                .image
                    width: 20px
                    height: 20px
                    vertical-align: middle
                    margin-right: 5px
    .support
        display: flex
        flex-direction: column  
        justify-content: center
        .item
            display: flex
            margin-bottom: 5px
            overflow: hidden
            white-space: normal
            .icon
                width: 18px
                height: 18px 
                vertical-align: margin-middle
                margin-right: 5px     
</style>