<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav"
                    @titleClick="titleClick"
                    ref="nav"></detail-nav-bar>
   <scroll class="content" ref="scroll"
           :probeType="3"
           @scroll="contentScroll">
<!--     <div>{{$store.state.cartList}}</div>-->
     <detail-swiper :top-images="topImages"></detail-swiper>
     <detail-base-info :goods="goods"></detail-base-info>
     <detail-shop-info :shop="shop"></detail-shop-info>
     <detail-goods-info :detail-info="detailInfo"
                        @goodsImgLoad="goodsImgLoad">
     </detail-goods-info>
     <detail-param-info :param-info="paramInfo" ref="param"></detail-param-info>
     <detail-comment-info :comment-info="commentInfo" ref="comment"></detail-comment-info>
     <goods-list :goods="recommends" ref="recommends"></goods-list>
   </scroll>
    <back-top @click.native="backTop" v-show="isShowTop"></back-top>
    <detail-bottom-bar @addCart="addToCart"></detail-bottom-bar>
  </div>
</template>
<script>
  import {getDetail, Goods, Shop, GoodsParam, getRecommend} from "network/detail";
  import DetailNavBar from "./childComps/DetailNavBar";
  import DetailSwiper from "./childComps/DetailSwiper";
  import DetailBaseInfo from "./childComps/DetailBaseInfo";
  import DetailShopInfo from "./childComps/DetailShopInfo";
  import DetailGoodsInfo from "./childComps/DetailGoodsInfo";
  import DetailParamInfo from "./childComps/DetailParamInfo";
  import DetailCommentInfo from "./childComps/DetailCommentInfo";
  import GoodsList from "components/content/goods/GoodsList";
  import DetailBottomBar from "./childComps/DetailBottomBar";
  import BackTop from "components/content/backTop/BackTop";
  import Scroll from "components/common/scroll/Scroll"
  export default {
    name: "Detail",
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      DetailGoodsInfo,
      DetailParamInfo,
      DetailCommentInfo,
      GoodsList,
      DetailBottomBar,
      BackTop,
      Scroll
    },
    data() {
      return {
        iid: null,
        topImages: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {},
        commentInfo: {},
        recommends: [],
        themeTopY: [],
        currentIndex: 0,
        isShowTop: false
      }
    },
    methods: {
      titleClick(index) {
        this.$refs.scroll.scrollTo(0, -this.themeTopY[index]+44, 200)
      },
      backTop() {
        this.$refs.scroll.scrollTo(0, 0)
      },
      goodsImgLoad() {
        this.$refs.scroll.refresh();
        this.themeTopY = [];
        this.themeTopY.push(0);
        this.themeTopY.push(this.$refs.param.$el.offsetTop);
        this.themeTopY.push(this.$refs.comment.$el.offsetTop);
        this.themeTopY.push(this.$refs.recommends.$el.offsetTop);
      },
      contentScroll(position) {
        const positionY = -position.y
        let length = this.themeTopY.length
        for(let i = 0; i < length; i++) {
          // if(positionY > this.themeTopY[i] && positionY < this.themeTopY[i+1]){
          //   console.log(i);
          // }
          if(this.currentIndex !==i && ((i < length - 1) && positionY >= this.themeTopY[i] && positionY < this.themeTopY[i+1]
            || (i === length - 1) && positionY >= this.themeTopY[i] )) {
            this.currentIndex = i
            this.$refs.nav.currentIndex = this.currentIndex
          }
        }
        this.isShowTop = (-position.y) > 1000
      },
      addToCart() {
        const product = {}
        product.image = this.topImages[0]
        product.title = this.goods.title
        product.desc = this.goods.desc
        product.price = this.goods.realPrice
        product.iid = this.iid
        this.$store.commit('addCart', product)
        // this.$store.dispatch('addCart', product).then(res => {
        //   console.log(res);
        // })
      }
    },
    mounted() {
    },
    updated() {
    },
    created() {
      this.iid = this.$route.params.iid
      getDetail(this.iid).then(res => {
        const data = res.result;
        this.topImages = data.itemInfo.topImages
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
        this.shop = new Shop(data.shopInfo)
        this.detailInfo = data.detailInfo
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)
        if (data.rate.cRate !== 0) {
          this.commentInfo = data.rate.list[0]
        }
      })
      getRecommend().then(res => {
        this.recommends = res.data.list
      })
    }
  }
</script>

<style scoped>
  #detail{
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }
  .detail-nav{
    position: relative;
    z-index: 9;
    background-color: #fff;
  }
  .content{
    height: calc(100% - 44px - 49px);
  }
</style>
