<template>
    <div class="cart-control">
        <div class="cart-decrease iconfont icon-remove_circle" 
             v-show="food.count>0" 
             @click.stop="decrease"
        >
        </div>
        <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
        <div class="cart-add iconfont icon-add_circle" 
             @click.stop="add"
        >
        </div>
    </div>
</template>
<script>
    export default {
        name: 'cart-control',
        props: {
            food: {
                type: Object
            }
        },
        methods: {
            decrease() {
                if (this.food.count) {
                    this.food.count--
                }
            },
            add(event) {
                if (!this.food.count) {
                    this.$set(this.food, 'count', 1)
                } else {
                    this.food.count++
                }
                this.$emit('add', event.target)
            }
        }
    }
</script>
<style lang="stylus" scoped>
    @import "~common/stylus/variable"
    .cart-control
        display: flex
        align-items: center
        .cart-decrease
            display: inline-block
            padding: 6px
            font-size: $fontsize-large-xx
            color: $color-light-grey
        .cart-count 
            width: 12px   
            line-height: 24px   
            text-align: center
            font-size: $fontsize-small-s
            color: $color-grey
        .cart-add 
            display: inline-block
            padding: 6px   
            font-size: $fontsize-large-xx
            color: $color-yellow
</style>
