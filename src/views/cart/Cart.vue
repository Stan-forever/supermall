<template>
  <div class="cart">
    <nav-bar class="cart-nav">
      <div slot="center">购物车({{length}})</div>
    </nav-bar>
    <cart-list></cart-list>
    <cart-bottom>
        <div>我是newbranch分支</div>
    </cart-bottom>
    
  </div>
</template>

<script>
import NavBar from "components/common/navbar/NavBar";
import CartList from "./childComps/CartList";
import CartBottom from "./childComps/CartBottom";
import { mapGetters } from 'vuex'
  export default {
    name: "Cart",
    components: {
      NavBar,
      CartList,
      CartBottom
    },
    computed: {
      // totalGoods() {
      //   return this.$store.state.cartList.length
      // }
      ...mapGetters({
        length: 'cartLength',
        list: 'cartList'
      })
    }
  }
</script>

<style scoped>
  .cart {
    height: 100vh;
  }
  .cart-nav{
    background-color: var(--color-tint);
    color: white;
  }
</style>
