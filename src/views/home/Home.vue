<template>
    <div id="home">
      <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>

      <scroll class="content" ref="scroll" :probe-type="3" @scroll="contentScroll">
        <home-swiper :banners="banners"/>
        <RecommendView :recommends="recommends"/>
        <feature-view/>
        <tab-control class="tab-control" :titles="['流行','新款','精选']" @tabClick="tabClick"/>
        <goods-list :goods="showGoods"></goods-list>
      </scroll>
      
      <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
    </div>
</template>

<script>
import HomeSwiper from './childrenComponents/HomeSwiper'
import RecommendView from './childrenComponents/RecommendView'
import FeatureView from './childrenComponents/FeatureView'

import NavBar from 'components/common/navbar/NavBar'
import TabControl from 'components/content/tabControl/TabControl'
import GoodsList from 'components/content/goods/GoodsList'
import Scroll from 'components/common/scroll/Scroll'
import BackTop from 'components/content/backTop/BackTop'

import {getHomeMultidata,getHomeGoods} from 'network/home'

export default {
  name:'Home',
  data(){
    return {
      banners: [],
      recommends: [],
      goods:{ //设计好数据模型
        'pop':{page:0,list:[]},
        'new':{page:0,list:[]},
        'sell':{page:0,list:[]},
      },
      currentType:'pop',
      isShowBackTop: false
    }
  },
  computed:{
    showGoods(){
      return this.goods[this.currentType].list
    }
  },
  created(){
    //1.请求多个数据
    this.getHomeMultidata()

    // 请求商品数据
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')
  },
  methods:{
    /**
     * 事件监听相关方法
     */
    tabClick(index) {
      switch (index) {
        case 0:
          this.currentType='pop'
          break;
        case 1:
            this.currentType='new'
            break;
        case 2:
            this.currentType='sell'
            break;
      }
    },
    backClick() {
      //访问内部的属性
      this.$refs.scroll.scrollTo(0,0,500)
    },
    contentScroll (position) { //从scroll组件中取出pisition
      this.isShowBackTop = -position.y > 1000 
    },
    /**
     * 网络请求相关方法
     */
    getHomeMultidata() { 
      getHomeMultidata().then(res => {
      // console.log(res)
      this.banners = res.data.banner.list
      this.recommends = res.data.recommend.list
    })
    },
    getHomeGoods(type){
      const page = this.goods[type].page + 1
      getHomeGoods(type,page).then(res => {
        console.log(res)
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1
      })
    }
  },
  components:{
    HomeSwiper,
    RecommendView,
    FeatureView,
    GoodsList,
    NavBar,
    TabControl,
    Scroll,
    BackTop
  },
};
</script>

<style scoped>
  #home {
    padding-top: 44px;
    height: 100vh; /* 100个视口*/
    position: relative;
  }
  .home-nav {
    background-color: var(--color-tint);
    color: #fff;
    
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }
  .tab-control {
    position: sticky;
    z-index: 9;
    top:44px;
    background: #fff;
  }

  /* 确定中间的高度 */
  .content {
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
  }

  /* 或者用calc */
  /* .content {
    height:calc(100% - 93px)
    overflow: hidden;
    margin-top:44px;
  } */
</style>

