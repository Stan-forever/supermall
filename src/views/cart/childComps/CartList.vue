<template>
  <div class="cart">
    <scroll class="cart-list" ref="scroll">
<!--      <cart-list-item v-for="(item, index) in cartList" :product="item" :key="index"></cart-list-item>-->
      <cart-list-item v-for="(item, index) in cartList" :item-info="item" :key="index"></cart-list-item>
    </scroll>
  </div>
</template>
<script>
import { mapGetters } from 'vuex'
import Scroll from "components/common/scroll/Scroll";
import CartListItem from "./CartListItem";
  export default {
    name: "CartList",
    components: {
      Scroll,
      CartListItem
    },
    computed: {
      ...mapGetters(['cartList'])
    },
    activated() {
      this.$refs.scroll.refresh()
    }
  }
</script>

<style scoped>
  .cart {
    height: calc(100% - 44px - 49px - 40px);
  }
  .cart-list {
    height: 100%;
    overflow: hidden;
  }
</style>
