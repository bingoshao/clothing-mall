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
  props:{
    probeType:{
      type:Number,
      default:0
    }
  },
  data() {
    return {
      scroll:null
    }
  },
  mounted(){
    // 1.创建better-scroll对象
    this.scroll = new BScroll(this.$refs.wrapper,{
      click:true,  //控制div等元素是否能被点击
      probeType:this.probeType //有的需要实时监听，有的页面不需要，所以设置一个动态的probeType传入进来
    })

    // 2.监听滚动的位置
    this.scroll.on('scroll',position => {
      this.$emit('scroll',position)
    })
  },
  methods:{
    //自己封装一个scrollTo,外部组件访问的时候就不需要拿到scroll属性而是直接拿到这个方法
    scrollTo(x,y,time=300) {
      this.scroll.scrollTo(x,y,time)
    }
  }
};
</script>

<style scoped>

</style>
