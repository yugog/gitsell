<template>
  <div class="shopCart">
    <div class="content">

      <div class="content-left" @click="showShop">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}" >
            <i class="icon-close" :class="{'highlight':totalCount>0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalPrice>0}">¥ {{totalPrice}}</div>
        <div class="desc">
          <span class="text">另需配送费¥{{deliveryPrice}}元</span>
        </div>
      </div>

      <div class="content-right" :class="payClass">
        <div class="pay" :class="payClass">
          {{payDesc}}
        </div>
      </div>

      <transition name="side">
      <div class="shop-list" v-show="listShow">
        <div class="shop-top">
          <h1 class="title">购物车</h1>
          <span class="clear" @click="clear">清空</span>
        </div>
        <div class="shop-content" ref="shopFoodList">
          <ul>
            <li class="food-box" v-for="food in selectFoods" >
              <h2 class="name">{{food.name}}</h2>
              <div class="price">¥{{food.price*food.count}}</div>
              <div class="control-wrapper">
                <control :food="food"></control>
              </div>
            </li>
            <li class="food-box"></li>
          </ul>
        </div>
      </div>
        </transition>
    </div>
    <transition name="fade">
    <div class="back" v-show="listShow" @click="showShop"></div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">

  import Control from 'components/control/control.vue';
  import BScroll from 'better-scroll';

  export default{
      props:{
          selectFoods:{
            type:Array,
            default(){
                return [{
                    count:0,
                  price:0
                }];
            }
          },
        deliveryPrice:{
          type:Number,
          default:0
        },
        minPrice:{
            type:Number,
            default:0
        }
      },
    data(){
      return{
        judge:false,
      }
    },
    computed:{
          totalPrice(){
              let total = 0;
              this.selectFoods.forEach((food) => {
                  total += food.price * food.count;
              });
              return total;
          },
      totalCount() {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc(){
              if (this.totalPrice === 0){
                  return "¥"+this.minPrice+"起送";
              }else if (this.totalPrice < this.minPrice){
                  let differrence = this.minPrice - this.totalPrice;
                  return "还差¥"+differrence+"元起送";
              }else {
                  return "去结算";
              }
      },
      payClass(){
          if (this.totalPrice >= this.minPrice){
              return "account";
          }else {
              return "";
          }
      },
      listShow(){
        this.$nextTick(() => {
          if (!this.scroll) {
            //获取父级
            this.scroll = new BScroll(this.$refs.shopFoodList, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
        let show = this.judge;
//alert(show)
          return show;
      }

    },
    methods:{
      showShop(){
        if (!this.totalCount){
          this.judge=false;
          return;
        }
          this.judge = !this.judge;



      },
      clear() {
        this.selectFoods.forEach((food) => {
          food.count = 0;
        });
      }

    },
    components:{
      control:Control
    }
  };
</script>

<style>
  .shopCart{
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 50;
    width: 100%;
    height: 48px;
    /*background: #141d27;*/
  }
    .shopCart .content{
      display: flex;
      background: #141d27;
    }

      .shopCart .content-left{  flex: 1; font-size: 0  }
        .shopCart  .logo-wrapper{
          display: inline-block;
          position: relative;
          top: -10px;
          margin: 0px 12px 0px 12px;
          padding: 6px;
          /*z-index: 51;*/
          width: 56px;
          height: 56px;
          box-sizing: border-box;
          vertical-align: top;
          border-radius: 50%;
          background: #141d27;;
          text-align: center;
        }
        .shopCart .content-left .logo-wrapper .logo{
          width:100%;
          height: 100%;
          border-radius: 50%;
          background: #2b343c;
        }
        .shopCart .content-left .logo-wrapper .logo.highlight{
    background: rgb(0,160,282);
  }
        .shopCart  .logo-wrapper .num{
          position: absolute;
          top: 0;
          right: 0;
          width: 24px;
          height: 16px;
          line-height: 16px;
          text-align: center;
          border-radius: 16px;
          font-size: 9px;
          font-weight: 700;
          color: #ffffff;
          background: rgb(240,20,20);
          box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
        }
          .icon-close{
            font-size: 24px;
            line-height: 44px;
            color: #80858a;
          }
          .shopCart .icon-close.highlight{
    color: #ffffff;
  }
        .shopCart .content-left .price{
          display: inline-block;
          margin: 12px 12px 12px 0px;
          font-weight: 700;
          font-size: 16px;
          line-height: 24px;
          color: #80858a;
        }
        .shopCart .content-left .price.highlight{
    color: #ffffff;
  }
        .shopCart .desc{
          display: inline-block;
          padding-left: 12px;
          border-left: 1px solid #2b343c;
          color: #80858a;
        }
          .shopCart .desc .text{
            font-size: 14px;
            font-weight: 300;
            line-height: 24px;
          }

      .shopCart .content-right{
        flex: 0 0 105px;
        padding: 10px 0px;
        width: 105px;
        box-sizing: border-box;
        vertical-align: top;
        text-align: center;
        background: #2b343c;
      }
      .shopCart .content-right.account{
        background: green;
      }
        .shopCart .content-right .pay{
          font-size: 14px;
          line-height: 30px;
          color: #80858a;
        }
          .shopCart .content-right .pay .price{font-weight: 700;}
          .shopCart .content-right .pay.account{
            color: #ffffff;
          }



  .side-enter-active {
    transition: all .4s linear;
  }
  .side-leave-active {
    transition: all 0.4s linear;
  }
  .side-enter,.side-leave-to {
    transform: translate3D(0,100%,0);
    opacity: 0;
  }

      .shop-list{
        position: absolute;
        left: 0;
        bottom: 48px;
        width: 100%;
        z-index: -1;

      }
        .shop-list .shop-top{
          position: relative;
          width: 100%;
          height: 40px;
          background: #f3f5f7;
          border-bottom: 1px solid rgba(7,17,270,0.1);
        }
          .shop-list .shop-top .title{
            position: absolute;
            left: 18px;
            font-size: 14px;
            font-weight: 300;
            line-height: 40px;
            color: rgb(7,17,27);
          }
          .shop-list .shop-top .clear{
            position: absolute;
            right: 18px;
            line-height: 40px;
            font-size: 12px;
            color: rgb(0,160,220);
          }

        .shop-list .shop-content{
          padding: 0px 18px;
          overflow: hidden;
          max-height: 200px;
          background: #ffffff;
        }
          .shop-list .shop-content .food-box{
            position: relative;
            padding: 12px 0;
            font-size: 0;
            border-bottom: 1px solid rgba(7,17,270,0.1);
          }
            .shop-list .shop-content .food-box .name{
              display: inline-block;
              line-height: 24px;
              font-size: 14px;
              color: rgb(7,17,27);
            ;
            }
            .shop-list .shop-content .food-box .price{
              position: absolute;
              right: 84px;
              bottom: 12px;
              line-height: 24px;
              font-size: 14px;
              color: rgb(240, 20, 20);
            }
            .shop-list .shop-content .food-box .control-wrapper{
              position: absolute;
              right: -8px;
              bottom: 0px;
              line-height: 16px;
            }




  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.3s;
  }
  .fade-enter, .fade-leave-to  {
    opacity: 0;
  }
  .back{
    position: fixed;
    top: 0px;
    left: 0px;
    z-index: -2;
    width: 100%;
    height: 100%;
    /*padding: 30px;*/
    background: rgba(0,0,0,0.9);
    /*backdrop-filter: blur(10px);*/

  }


</style>
