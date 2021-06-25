<template>
  <div id="detailnav">
    <nav-bar>
      <div slot="left" class="backicon">
        <img src="~assets/img/common/back.svg" alt="" @click="back">
      </div>
      <div slot="center" class="titles">
        <span v-for="(title,index) in titles"
              class="title"
              :class="{active: index === currentIndex}"
              @click="titlecolor(index)">{{title}}</span>
      </div>
    </nav-bar>
  </div>
</template>
<script>
import NavBar from "components/common/navbar/NavBar";
  export default {
    name: "DetailNavBar",
    data() {
      return {
        titles: ['商品', '参数', '评论', '推荐'],
        currentIndex: 0
      }
    },
    components: {
      NavBar
    },
    methods: {
      titlecolor(index) {
        this.currentIndex = index
        this.$emit('titleClick', index)
      },
      back() {
        this.$router.back()
      }
    }
  }
</script>

<style scoped>
  .titles{
    display: flex;
  }
  .title{
    flex: 1;
  }
  .active{
    color: var(--color-high-text);
  }
  .backicon img{
    margin-top: 10px;
  }
</style>
