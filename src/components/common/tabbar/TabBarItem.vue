<template>
 <div class="tab-bar-item" @click="itemClick">
   <div v-if="!isActive"><slot name="item-icon"></slot></div>
   <div v-else><slot name="item-icon-active"></slot></div>
   <div :style="activeStyle"><slot name="item-text"></slot></div>
 </div>
</template>
<script>
export default {
  name: "TabBarItem",
  props: {
    path: String,
    activecolor: {
      type: String,
      default: 'red'
      }
    },
  computed: {
    isActive() {
      return this.$route.path.indexOf(this.path) !== -1
    },
    activeStyle() {
      return this.isActive ? {color : this.activecolor } : {}
    }
  },
  methods:{
    itemClick() {
      this.$router.replace(this.path).catch(error => {return error})
    }
  }
}
</script>
<style scoped>
  .tab-bar-item{
    flex: 1;
    text-align: center;
    height: 49px;
    font-size: 14px;
  }
  .tab-bar-item img{
    height: 24px;
    width: 24px;
    margin-top: 3px;
    margin-bottom: 2px;
    vertical-align: middle;
  }
  .active{
    color: #9fc6ff;
  }
</style>
