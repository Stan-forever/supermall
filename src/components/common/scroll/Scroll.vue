<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  export default {
    name: "Scroll",
    data() {
      return {
        scroll: null
      }
    },
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUpLoad: {
        type: Boolean,
        default: false
      }
  },
    mounted() {
      this.scroll =  new BScroll(this.$refs.wrapper, {
        observeDOM: true,
        // observeImage: true,
        click: true,
        probeType: this.probeType,
        pullUpLoad: this.pullUpLoad
      })
      this.scroll.on('scroll', (position) => {
        this.$emit('scroll', position)
      })
      if(this.pullUpLoad) {
        this.scroll.on('pullingUp', () => {
          this.$emit('pulling')
        })
      }
    },
    methods: {
      scrollTo(x, y, time= 1000){
        this.scroll && this.scroll.scrollTo(x, y, time)
      },
      finishPullUp() {
        this.scroll.finishPullUp()
      },
      refresh() {
        this.scroll && this.scroll.refresh()
      },
      getY() {
        return this.scroll ? this.scroll.y : 0
      }
    }
  }
</script>
<style scoped>
</style>

