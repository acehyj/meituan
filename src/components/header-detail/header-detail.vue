<template>
    <div v-show="visible" class="header-detail" @click="click">
        <transition name="slide">
            <div class="detail-wrapper" @click.stop>
                <div class="support">
                    <div class="item"  
                         v-for="item in seller.support" 
                         :key="item.type"
                    > 
                        <img :src="item.icon" alt="" class="icon">
                        <span>{{item.description}}</span>
                    </div>
                </div>
                <div class="delivery">
                    <div class="title">配送</div>
                    <div class="content">
                        <div class="minPrice">起送￥{{seller.minPrice}}</div>
                        <div class="shipping_time">配送时间{{seller.shipping_time}}</div>
                    </div>
                </div>
                <div class="bulletin">
                    <div class="title">公告</div>
                    <div class="content">{{seller.bulletin}}</div>
                </div>
            </div>
        </transition>
    </div>
</template>
<script>
    import popupMixin from 'common/mixins/popup'
    export default {
        name: 'header-detail',
        props: {
            seller: {
                type: Object,
                default() {
                    return {}
                }
            }
        },
        data() {
            return {
                visible: false
            }
        },
        mixins: [popupMixin],
        methods: {
            click() {
                this.hide()
            }
        }
    }
</script>
<style lang="stylus" scoped>
    @import "~common/stylus/variable"
    .header-detail
        position: fixed
        z-index: 100
        top: 0
        left: 0
        width: 100%
        height: 100%
        overflow: auto
        backdrop-filter: blur(10px)
        opacity: 1
        color: $color-grey
        background: $color-background-ss
        
        .detail-wrapper
            position: absolute
            bottom: 0
            width: 100%
            height: 266px
            padding: 20px 15px 35px
            box-sizing: border-box
            background: $color-white
            color: $color-dark-grey
            font-size: $fontsize-small
            &.slide-enter-active, &.slide-leave-active {
                transition: all 1s linear;
            }

            &.slide-enter, &.slide-leave-to {
                transform: translate3d(0, 100%, 0);
            }
            .support
                display: flex
                flex-direction: column  
                justify-content: center
                padding-bottom: 15px
                .item
                  display: flex
                  margin-bottom: 5px
                  overflow: hidden
                  white-space: normal
                  .icon
                      width: 15px
                      height: 15px 
                      vertical-align: bottom
                      margin-right: 5px    
            .delivery
                padding-bottom: 15px
                .title
                    font-weight: bold
                    font-size: $fontsize-large
                    padding-bottom: 2px
                .minPrice
                    padding-bottom: 2px
            .bulletin
                overflow: hidden
                .title
                    font-weight: bold
                    font-size: $fontsize-large
                    padding-bottom: 2px

</style>
