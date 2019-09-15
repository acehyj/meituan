<template>
    <cube-scroll ref="scroll" class="ratings" :options="scrollOptions" :data="ratings">
        <div class="ratings-content">
            <div class="overview">
                <div class="overview-left">
                    <h1 class="score">{{seller.score}}</h1>
                    <div class="title">商家评分</div>
                </div>
                <div class="overview-middle">
                    <div class="score-wrapper">
                        <span class="title">口味</span>
                        <star :score="seller.tasteScore"></star>
                        <span class="score">{{seller.tasteScore}}</span>
                    </div>
                    <div class="score-wrapper">
                        <span class="title">包装</span>
                        <star :size="36" :score="seller.packScore"></star>
                        <span class="score">{{seller.packScore}}</span>
                    </div>
                </div>
                <div class="overview-right">
                     <h1 class="score">{{seller.deliveryScore}}</h1>
                    <div class="title">配送评分</div>
                </div>
            </div>
            <div class="split"></div>
            <rating-select :ratings="ratings" :selectType="selectType" :onlyContent="onlyContent" @select="onSelect" @toggle="onToggle"></rating-select>
            <div class="rating-wrapper" v-show="computedRatings.length">
                <ul>
                    <li class="rating-item border-bottom-1px" v-for="(rating,index) in computedRatings" :key="index">
                        <div class="avatar">
                            <img :src="rating.avatar" width="28" height="28">
                        </div>
                        <div class="content">
                            <h1 class="name">{{rating.username}}</h1>
                            <div class="delivery">{{rating.deliveryTime}}分钟送达</div>
                            <p class="text">{{rating.text}}</p>
                            <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                                <span class="iconfont icon-thumb_up"></span>
                                {{rating.recommend.toString()}}
                            </div>
                            <div class="time">
                                {{rating.rateTime}}
                            </div>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </cube-scroll>
</template>
<script>
    import Star from 'components/star/star'
    import RatingSelect from 'components/rating-select/rating-select'
    import { getRatings } from 'api'
    import moment from 'moment'
    const ALL = 2
    // 
    export default {
        name: 'ratings',
        props: {
            data: {
                type: Object
            }
        },

        data() {
            return {
                onlyContent: false,
                selectType: ALL,
                ratings: [],
                scrollOptions: {
                    click: false,
                    directionLockThreshold: 0
                }
            }
        },
        computed: {
            seller() {
                return this.data.seller
            },
            computedRatings() {
              let ret = []
              this.ratings.forEach((rating) => {
                if (this.onlyContent && !rating.text) {
                  return
                }
                if (this.selectType === ALL || rating.rateType === this.selectType) {
                  ret.push(rating)
                }
              })
              return ret
            }
        },
        components: {
            Star,
            RatingSelect
        },
        methods: {
            format(time) {
                return moment(time).format('YYYY-MM-DD hh:mm')
            },
            onSelect(type) {
              this.selectType = type
            },
            onToggle() {
              this.onlyContent = !this.onlyContent
            },
            fetch() {
                getRatings().then((ratings) => {
                    this.ratings = ratings
                })
            }
  
        }
    }
</script>
<style lang="stylus" scoped>
@import '~common/stylus/variable'
.ratings
    position: relative
    white-space: normal
    height: 100%
    .overview
        display: flex
        padding: 15px 0
        .overview-left 
            flex: 0 0 85px
            width: 80px
            display: flex
            flex-direction: column
            justify-content: center
            align-items: center
            text-align: center

            @media only screen and (max-width: 320px) {
                flex: 0 0 120px;
                width: 120px;
            }
            .score
                margin-bottom: 6px
                line-height: 28px
                font-size: $fontsize-large-xxx
                color: $color-yellow
            .title 
                margin-bottom: 8px
                line-height: 12px
                font-size: $fontsize-small
                color: $color-dark-grey

        .overview-middle
            flex: 1
            display: flex
            flex-direction: column
            justify-content: center
            align-items: center

            @media only screen and (max-width: 320px) {
                padding-left: 6px
            }

            .score-wrapper 
                display: flex
                align-items: center
                margin-bottom: 8px
                .title 
                    line-height: 18px
                    font-size: $fontsize-small
                    color: $color-dark-grey
                .star 
                    margin: 0 12px
                .score 
                    line-height: 18px
                    font-size: $fontsize-small
                    color: $color-yellow
        
        .overview-right 
            flex: 0 0 85px
            padding: 6px 0
            width: 85px
            border-left: 1px solid $color-col-line
            text-align: center

            @media only screen and (max-width: 320px) {
                flex: 0 0 120px
                width: 120px
            }

            .score 
                margin-bottom: 6px
                line-height: 28px
                font-size: $fontsize-large-xxx
                color: $color-light-grey
            .title 
                margin-bottom: 8px
                line-height: 12px
                font-size: $fontsize-small
                color: $color-light-grey
          
    .rating-wrapper 
        padding: 0 18px
        .rating-item 
            display: flex
            padding: 18px 0 10px 0

            &:last-child 
                border-none()
            .avatar 
                flex: 0 0 28px
                width: 28px
                margin-right: 12px
                img 
                    height: auto
                    border-radius: 50%
            .content 
                position: relative
                flex: 1
                .name 
                    margin-bottom: 4px
                    line-height: 12px
                    font-size: $fontsize-large-xx
                    color: $color-dark-grey
                    margin-bottom: 7px
                .delivery
                    font-size: $fontsize-small
                    color: $color-grey
                .text 
                    margin: 14px 0
                    line-height: 20px
                    color: $color-dark-grey
                    font-size: $fontsize-large
                    display: flex
                    align-items: center
                    flex-wrap: wrap
                .recommend
                    font-size: $fontsize-small
                    color: $color-light-grey
                .time 
                    position: absolute
                    top: 0
                    right: 0
                    line-height: 12px
                    font-size: $fontsize-small
                    color: $color-light-grey
                
</style>