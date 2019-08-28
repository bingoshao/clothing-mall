<template>
  <div class="tab-bar-item" @click="itemClick">
    <!-- 插槽会被替换 都要用div包裹-->
    <div v-if="!isActive">
      <slot name="item-icon"></slot>
    </div>
     
    <div v-else>
      <slot name="item-icon-active"></slot>
    </div>

    <div :style="activeStyle">
      <slot name="item-text"></slot>
    </div>
    
  </div>    
</template>

<script>
export default {
  props:{
    path:String,
    //动态传入颜色
    activeColor:{
      type:String,
      default:'#d4237a'
    }
  },
  data() {
    return {
      // isActive:true
    }
  },
  computed: {
    isActive(){
      // 哪个路由处于活跃，route就是哪个路由
      return this.$route.path.indexOf(this.path) !== -1
    },
    //动态绑定style
    activeStyle() {
      return this.isActive?{color:this.activeColor} : {}
    }
  },
  methods: {
    itemClick() {
     this.$router.replace(this.path)
    }
  },
};
</script>

<style scoped>
/* 均等分,不同于space-between */
.tab-bar-item {
  flex: 1; 
  text-align: center;
  height: 49px;
  font-size: 14px;
}
.tab-bar-item img {
  width: 24px;
  height:24px;
  margin-top: 3px;
  vertical-align: middle;
  margin-bottom: 2px;
}

</style>
