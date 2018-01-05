<template>
  <div class="control">
    <transition name="decr">
    <div class="control-decrease icon-shopping_cart" v-show="food.count>0" @click="decreaseCount"></div>
    </transition>
    <transition name="fade">
    <div class="control-count" v-show="food.count>0">{{food.count}}</div>
    </transition>
    <div class="control-add icon-thumb_down" @click="addCount"></div>

  </div>
</template>

<script type="text/ecmascript-6">

  import Vue from 'vue'

  export default{
      props:{
          food:{
              type:Object
          }
      },
    created(){
//          console.log(this.food);
    },
    methods:{
        //在goods.vue foodsScroll 添加click：true 因为用了better-scroll
        addCount(){
            if (!this.food.count){
              //无效 需要import引入vue
//                this.food.count = 1;
              Vue.set(this.food,'count',1);

            }else {
              this.food.count++;
            }
        },
      decreaseCount(){
        if (this.food.count){
          this.food.count--;
        }
      }
    }
  };
</script>

<style>

  .decr-enter-active {
    transition: all .7s linear;
  }
  .decr-leave-active {
    transition: all 0.3s ;
  }
  .decr-enter,.decr-leave-to {
    transform: translate3D(10px,0,0);
    opacity: 0;
  }
  .control{
    text-align: center;
    padding: 6px;
    font-size: 0;
  }
    .control .control-decrease{
      display: inline-block;
      line-height: 24px;
      font-size: 24px;
      color: #1aa1d9;
      /*transform: rotate(180deg);*/
    }
    .control .control-count{
      vertical-align: top;
      display: inline-block;
      line-height: 24px;
      width: 24px;
      height: 24px;
      font-size: 10px;
      color: #80858a;
    }
    .control .control-add{
      display: inline-block;
      /*vertical-align: top;*/
      line-height: 24px;
      font-size: 24px;
      color: #1aa1d9;
    }

</style>
