<template>
    <div class="rating-select">
        <div class="rating-type border-bottom-1px">
            <span @click="select(2)" class="block" :class="{'active': selectType === 2}">
                {{desc.all}}<span class="count">({{ratings.length}})</span>
            </span>
            <span @click="select(0)" class="block" :class="{'active': selectType === 0}">
                {{desc.positive}}<span class="count">({{positives.length}})</span>
            </span>
            <span @click="select(1)" class="block" :class="{'active': selectType === 1}">
                {{desc.negative}}<span class="count">({{negatives.length}})</span>
            </span>
        </div>
    </div>
</template>
<script>
    const POSITIVE = 0 // 好评
    const NEGATIVE = 1 // 差评
    const ALL = 2 // 全部
    
    const EVENT_TOGGLE = 'toggle'
    const EVENT_SELECT = 'select'
    export default {
        props: {
            ratings: {
                type: Array,
                default() {
                    return []
                }
            },
            selectType: {
                type: Number,
                default: ALL
            },
            onlyContent: {
                type: Boolean,
                default: false
            },
            desc: {
                type: Object,
                default() {
                    return {
                        all: '全部',
                        positive: '好评',
                        negative: '差评'
                    }
                }
            }
        },
        computed: {
            positives() {
                return this.ratings.filter((rating) => {
                    return rating.rateType === POSITIVE
                })
            },
            negatives() {
                return this.ratings.filter((rating) => {
                    return rating.rateType === NEGATIVE
                })
            }
        },
        methods: {
            select(type) {
                this.$emit(EVENT_SELECT, type)
            }
        }
    }
</script>
<style lang="stylus" scoped>
@import '~common/stylus/variable';

.rating-select 
    .rating-type 
        padding: 18px 0 10px 0
        margin: 0 18px
    .block 
        display: inline-block
        padding: 5px 8px
        margin-right: 8px
        line-height: 16px
        border: 1px solid $color-light-gray
        border-radius: 20px
        font-size: $fontsize-small
        color: $color-grey
        &.active 
            color: $color-yellow
            border: 1px solid $color-yellow
        .count 
            margin-left: 2px

</style>