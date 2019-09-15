<template>
    <div class="star">
        <span v-for="(itemClass, index) in itemClasses" :class="itemClass" class="star-item" :key="index"></span>
    </div>
</template>
<script>
    const LENGTH = 5
    const CLS_ON = 'on'
    const CLS_HALF = 'half'
    const CLS_OFF = 'off'
    export default {
        props: {
            score: {
                type: Number
            }
        },
        computed: {
            itemClasses() {
                let result = []
                const score = Math.floor(this.score * 2) / 2
                const hasDecimal = score % 1 !== 0
                const integer = Math.floor(score)
                for (let i = 0; i < integer; i++) {
                    result.push(CLS_ON)
                }
                if (hasDecimal) {
                    result.push(CLS_HALF)
                }
                while (result.length < LENGTH) {
                    result.push(CLS_OFF)
                }
                return result
            }
        }
    }
</script>
<style lang="stylus" scoped>
.star 
    display: flex
    align-items: center
    justify-content: center
    .star-item 
        background-repeat: no-repeat
        width: 15px
        height: 15px
        margin-right: 6px
        background-size: 15px 15px
        &:last-child 
            margin-right: 0;
        &.on 
            background-image: url('star_on.png')
        &.half 
            background-image: url('star_half.png')
        &.off 
            background-image: url('star_off.png')
</style>