<template>
    <div class="header" @click="detailShow">
      <!-- 头部文字信息 -->
      <div class="content-wrapper">
        <div class="avatar">
          <img :src="seller.avatar" height="64" width="64">
        </div>
        <div class="content">
          <div class="name">{{seller.name}}</div>
          <div class="description">
            {{seller.deliveryTime}}分钟 | {{seller.distance}}km
          </div>
          <div class="bulletin">{{seller.bulletin}}</div>
        </div>
      </div>
      <!-- 头部轮播图 -->
      <div class="support" @click="detailShow">
        <div class="slide">
          <cube-slide ref="slide" 
                      :data="seller.support"
                      :showDots=false
                      direction="vertical"
                      >
            <cube-slide-item v-for="(item,index) in seller.support" :key="index">
                <img :src="item.icon" alt="" class="icon">
                <span>{{item.description}}</span>
            </cube-slide-item>
          </cube-slide>
        </div>
        <span class="iconfont icon-arrow_right"></span>
      </div>
  </div>
</template>
<script>
    export default {
        props: {
            seller: {
                type: Object,
                default() {
                    return {}
                }
            }
        },
        methods: {
            detailShow() {
                this.headerDetailComp = this.headerDetailComp || this.$createHeaderDetail({
                    $props: {
                        seller: 'seller'
                    }
                })
                this.headerDetailComp.show()
            }
        }
    }
</script>
 
<style lang="stylus" scoped>
@import "~common/stylus/variable"
    .header
        position: relative
        overflow: hidden
        color: #fff
        background: $color-background-ss
        .content-wrapper
            position: relative
            padding: 20px 10px 10px 20px
            font-size: 0
            .avatar
                vertical-align: top
                display: inline-block
                img
                    border-radius: 2px
            .content
                display: inline-block
                width: 250px
                margin-left: 16px 
                font-size: $fontsize-medium
                .name
                    margin-bottom: 8px
                    font-size: $fontsize-large
                    font-weight: bold
                    line-height: 18px
                .description
                    font-size: $fontsize-small
                    margin-bottom: 10px
                .bulletin
                  width: 100%
                  overflow: hidden
                  white-space: nowrap
                  text-overflow: ellipsis
                  font-size: $fontsize-small
        .support
          position: relative
          botton: 0
          width: 100%
          height: 24px
          line-height: 24px
          overflow: hidden
          background: rgba(0, 0, 0, 0.2)
          text-align: center
          border-radius: 8px
          font-size: $fontsize-small
          .slide
            height: 100%
            .icon
              width: 15px
              height: 15px
              margin-right: 5px
              vertical-align: middle
          .icon-arrow_right
            position: absolute
            top: 0
            right: 0
            width: 30px
            margin-right: 2px
            line-height: 24px
</style>
