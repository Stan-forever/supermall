<template>
  <div class="bottom-menu">
    <CheckButton class="select-all" :is-checked="isSelectAll"></CheckButton>
    <span @click="checkBtnClick">全选</span>
    <span class="total-price">合计: ¥{{totalPrice}}</span>
    <span class="buy-product">去计算({{checkLength}})</span>
  </div>
</template>

<script>
import CheckButton from "components/content/button/CheckButton";
  export default {
    name: "CartBottom",
    components: {
      CheckButton
    },
    computed: {
      totalPrice() {
        const cartList = this.$store.state.cartList;
        return cartList.filter(item => {
          return item.isChecked
        }).reduce((preValue, item) => {
          return preValue + item.count* item.price
        }, 0).toFixed(2)
      },
      checkLength() {
        return this.$store.state.cartList.filter(item => item.isChecked).length
      },
      isSelectAll() {
        if(this.$store.state.cartList.length === 0) return false
        for (let item of this.$store.state.cartList) {
          if(!item.isChecked) {
            return false
          }
        }
        return true
      },

    },
    methods: {
      checkBtnClick: function () {
        // 1.判断是否有未选中的按钮
        let isSelectAll = this.$store.state.cartList.find(item => !item.isChecked);

        // 2.有未选中的内容, 则全部选中
        if (isSelectAll) {
          this.$store.state.cartList.forEach(item => {
            item.isChecked = true;
          });
        } else {
          this.$store.state.cartList.forEach(item => {
            item.isChecked = false;
          });
        }
      }
    }
  }
</script>

<style scoped>
  .bottom-menu {
    width: 100%;
    background-color: #eee;
    position: fixed;
    bottom: 50px;
    left: 0;
    box-shadow: 0 -2px 3px rgba(0, 0, 0, .2);
    font-size: 14px;
    color: #888;
    line-height: 44px;
    padding-left: 35px;
    box-sizing: border-box;
  }
  .bottom-menu .select-all {
    position: absolute;
    line-height: 0;
    left: 12px;
    top: 13px;
  }
  .bottom-menu .total-price {
    margin-left: 15px;
    font-size: 16px;
    color: #666;
  }
</style>
