<template>
    <div class="tab">
        <cube-tab-bar ref="tabbar"
                      :data="tabs"
                      v-model="selectedLabel" 
                      show-slider 
                      :useTransition="false" 
        >
        </cube-tab-bar>
        <div class="slide-wrapper">
            <cube-slide ref="slide" 
                        :data="tabs" 
                        :initial-index="index" 
                        :show-dots="false" 
                        :loop="false" 
                        :auto-play="false" 
                        :options="slideOptions" 
                        @change="changePage" 
                        @scroll="scroll">
                <cube-slide-item v-for="tab in tabs" :key="tab.label">
                    <component :is="tab.component" :data="tab.data" ref="component"></component>
                </cube-slide-item>
            </cube-slide>
        </div> 
    </div>
</template>
<script>
    export default {
        name: 'tab',
        props: {
            tabs: {
                type: Array,
                default() {
                    return {}
                }
            }
        },
        data() {
            return {
                index: 0,
                slideOptions: {
                    listenScroll: true, // 是否监控scroll事件
                    probeType: 3, // 0 不派发scroll事件，1：非实时；2：滑动过程中；3：不仅在屏幕滑动的过程中，而且momentum 滚动动画运行过程中实时派发
                    directionLockThreshold: 0, //???
                    eventPassthrough: 'vertical' 
                }
            }
        },
        computed: {
            selectedLabel: {
                get() {
                    return this.tabs[this.index].label
                },
                set(newval) { //newval为被点击的tab的label
                    this.index = this.tabs.findIndex((value) => {
                        return value.label === newval
                    })
                }
            }
        },
        mounted() {
            this.changePage(this.index)
        },
        methods: {
            changePage(current) {
                this.index = current
                const component = this.$refs.component[current]
                component.fetch && component.fetch() 

            },
            // 实时派发滚动的距离,如果使用了就把默认的useTransition属性设置为false,要不然会有冲突
            scroll(pos) {
                const tabbarWidth = this.$refs.tabbar.$el.clientWidth
                const slideWidth = this.$refs.slide.slide.scrollerWidth
                const transform = -pos.x / slideWidth * tabbarWidth
                this.$refs.tabbar.setSliderTransform(transform)
            }
        }
    }
</script>

<style lang="stylus" scoped>
.tab 
    display: flex
    flex-direction: column
    height: 100%

    >>> .cube-tab 
        padding: 15px 0

    .slide-wrapper 
        flex: 1;
        overflow: hidden
    

</style>
