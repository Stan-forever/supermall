<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
   <scroll class="content"
           ref="scroll"
           @scroll="contentScroll"
           :probe-type="3"
           @pulling="loadMore"
           :pull-up-load="true">
     <home-swiper :banners="banners" @swiperLoad="swiperLoad"></home-swiper>
     <recommend-view :recommends="recommends"></recommend-view>
     <feature-view></feature-view>
     <tab-control class="tab-control"
                  :titles="['流行', '新款', '精选']"
                  @tabClick="tabClick"
                  ref="tabcontrol"/>
                  <!-- @tabClick即v-on，用以绑定事件    -->
     <goods-list :goods="showGoods"/>
   </scroll>
    <back-top @click.native="backTop" v-show="isShowTop"></back-top>
  </div>
</template>

<script>
  import HomeSwiper from "./childComps/HomeSwiper";
  import RecommendView from "./childComps/RecommendView";
  import FeatureView from "./childComps/FeatureView";

  import NavBar from "components/common/navbar/NavBar";
  import TabControl from "components/content/tabControl/TabControl";
  import GoodsList from "components/content/goods/GoodsList";
  import Scroll from "components/common/scroll/Scroll";
  import BackTop from "components/content/backTop/BackTop";

  import {getHomeMultidata, getHomeGoods} from "network/home";
  import {debounce} from "common/utils";

  export default {
    name: "Home",
    components: {
      HomeSwiper,
      RecommendView,
      FeatureView,
      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      BackTop,
    },
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop' :{page: 0,list: []},
          'new' :{page: 0,list: []},
          'sell' :{page: 0,list: []},
        },
        currentType: 'pop',
        isShowTop: false,
        tabTop: 0,
        saveY: 0
      }
    },
    computed : {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    created() {
      this.getHomeMultidata()
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    activated() {
      this.$refs.scroll.refresh()
      this.$refs.scroll.scrollTo(0, this.saveY, 1)
    },
    deactivated() {
      this.saveY = this.$refs.scroll.getY()
    },
    mounted() {
      const refresh = debounce(this.$refs.scroll.refresh,1)
      this.$bus.$on('itemImgLoad', () => {
        // this.$refs.wrapper.refresh();
        refresh()
      })
    },
    methods: {
      swiperLoad() {
        this.tabTop = this.$refs.tabcontrol.$el.offsetTop;
        // console.log(this.$refs.tabcontrol.$el.offsetTop);
      },
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          this.banners = res.data.banner.list
          this.recommends = res.data.recommend.list
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1
          this.$refs.scroll.finishPullUp()
        })
      },
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
      },
      backTop() {
        this.$refs.scroll.scrollTo(0, 0)
      },
      contentScroll(position) {
        // console.log(position);
        this.isShowTop = (-position.y) > 1000
      },
      loadMore() {
        this.getHomeGoods(this.currentType)
        this.$refs.scroll.scroll.refresh()
      }
    }
  }
</script>

<style scoped>
  #home {
    height: 100vh;
    position: relative;
  }
  .home-nav{
    background-color: var(--color-tint);
    color: white;
    position: fixed;
    left: 0;
    top: 0;
    right: 0;
    z-index: 9;
  }
  .tab-control {
    position: sticky;
    top: 44px;
    z-index: 9;
  }
  .content{
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
</style>
