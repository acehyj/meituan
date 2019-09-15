<template>
    <span class="bubble">
        {{num}}
    </span>
</template>
<script>
    export default {
        name: 'bubble',
        props: {
            num: {
                type: Number
            }
        }
    }
</script>
<style lang="stylus" scoped>
@import '~common/stylus/variable';

.bubble 
    display: inline-block
    height: 12px
    line-height: 12px
    padding: 1px 3px
    text-align: center
    border-radius: 16px
    font-weight: 700
    font-size: $fontsize-small-ss
    font-family: Helvetica
    color: $color-white;
    background: $color-red
</style>