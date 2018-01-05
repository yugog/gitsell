<template>
  <transition name="sidefood">

  <div class="food" v-show="showFood" ref="food">
    <div class="food-box">
      <div class="images-top">
      <img :src="food.image">
    </div>
      <span class="icon-add_circle" @click="out"></span>
      <div class="body-wrapper">
      <div class="food-information">
          <h1 class="name">{{food.name}}</h1>
          <div class="desc">
          <span>月售{{food.sellCount}}份 好评率{{food.rating}}%</span>
        </div>
          <div class="price">
            ¥ <span class="price-num">{{food.price}}</span>
          </div>
          <div class="buy-car" @click="addCount" v-show="food.count == 0 || !food.count">
            <span class="text">加入购物车</span>
        </div>
        <div class="control-wrapper">
          <control :food="food"></control>
        </div>

      </div>
      <div class="food-introduce">
        <h2 class="title">商品介绍</h2>
        <p class="food-info">{{food.info}}</p>
      </div>
      <div class="food-ratings">
        <h2 class="title">商品评价</h2>
        <div class="rateType-wraper">
          <div class="all">全部{{all}}</div>
          <div class="good">推荐{{good}}</div>
          <div class="bad">吐槽{{bad}}</div>
        </div>
        <div class="ratings-body">
          <ul>
            <li v-for="rating in food.ratings" class="ratings-wrapper">
              <div rating-content>
                <span class="time">{{rating.rateTime | formatDate}}</span>
                <div class="user">
                  <span>{{rating.username}}</span>
                  <img width=12px height=12px :src="rating.avatar" ></div>
              </div>
              <span class="tall">{{rating.text}}</span>
            </li>
          </ul>
        </div>

      </div>
    </div>
    </div>
  </div>

    </transition>
</template>

<script type="text/ecmascript-6">

  import BScroll from 'better-scroll';
  import Vue from 'vue';
  import Control from 'components/control/control.vue';
  import {formatDate} from '../../common/js/date';

  export default{
      props:{
          food:{
              type:Object
          }
      },
    data(){
          return{
            showFood: false,
            all:{},
            good:{},
            bad:{}
          }
    },
    computed:{
    },
    methods:{
          show(){
            this.showFood = true;
            this.$nextTick(() => {
              if (!this.scroll) {
                this.scroll = new BScroll(this.$refs.food, {
                  click: true
                });
              } else {
                this.scroll.refresh();
              }
            });
          },
          out(){
              this.showFood =false;
              },
      //需要从good.vue传food进来
      _findAll(food){
        let goodnum = 0;
        let badnum = 0;
        food.ratings.forEach((rating) => {
          if (rating.rateType == 0){
            goodnum++;
            console.log("goodnum:"+goodnum);
          }else{
            badnum++;
            console.log("badnum:"+badnum);
          }
        });
        this.good = goodnum;
        this.bad = badnum;
        this.all = goodnum + badnum;
      },

      addCount(){
            //无效 需要import引入vue
  //                this.food.count = 1;
            Vue.set(this.food,'count',1);
      }
    },
    filters: {
      formatDate(time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    },
    components:{
      control:Control
    }
    // mounted(){
    //   this.$nextTick(() => {
    //     this._findAll();
    //   });
    // }
  };
</script>

<style>
  .sidefood-enter-active {
    transition: all .2s linear;
  }
  .sidefood-leave-active {
    transition: all 0.2s linear;
  }
  .sidefood-enter,.sidefood-leave-to {
    transform: translate3D(100%,0,0);
    opacity: 0.5;
  }
  .food{
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    z-index: 30;
    width: 100%;
    background: #ffffff;
  }
    .food .images-top{
      position: relative;
      width: 100%;
      height: 0;
      padding-top: 100%
      /*background-size: ;*/
    }
      .food .images-top img{
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        /*height: 325px;*/
      }
    .food .icon-add_circle{
      position: absolute;
      top: 10px;
      left: 10px;
      padding: 5px 5px;
      font-weight: 700;
      color: rgba(255,255,255,0.8);
    }

    .food .body-wrapper{
      background: #f3f5f7;
    }
      .food .body-wrapper .food-information,.food .body-wrapper .food-introduce,.food .body-wrapper .food-ratings{
        position: relative;
        padding: 18px 18px;
        background: #ffffff;
        margin-bottom: 16px;
      }
         .food .body-wrapper .food-information .name{
          font-size: 14px;
          font-weight: 700;
          color: rgb(7,17,27);
          line-height: 14px;
          margin-bottom: 8px;
        }
         .food .body-wrapper .food-information .desc{
          font-size: 10px;
          color: rgb(147,153,159);
          line-height: 10px;
        }
         .food .body-wrapper .food-information .price{
          /*display: inline-block;*/
          margin-top: 18px;
          font-size: 10px;
          line-height: 24px;
          color: rgb(220,10,10);
        }
          .food .body-wrapper .food-information .price-num{
            font-size: 16px;
            font-weight: 700;
          }
        .food .body-wrapper .food-information .buy-car{
          /*display: inline-block;*/
          position: absolute;
          bottom: 18px;
          right: 18px;
          padding: 10px 12px;
          z-index: 25;
          line-height: 12px;
          font-size: 10px;
          background: #1aa1d9;
          color: #ffffff;
          text-align: center;
          border-radius: 16px;
        }
        .food .body-wrapper .food-information .control-wrapper{
          position: absolute;
          bottom: 18px;
          right: 18px;
          z-index: 10;
        }
        .food .body-wrapper .food-introduce .title{
          margin-bottom: 6px;
          font-size: 16px;
          line-height: 16px;
      }
        .food .body-wrapper .food-introduce .food-info{
          padding: 0px 8px;
          line-height: 24px;
          font-size: 12px;
          font-weight: 300;
          color: rgb(77,85,93);
        }

      .food .body-wrapper .food-ratings .rateType-wraper{
        font-size: 0px;
      }
        .food .body-wrapper .food-ratings .rateType-wraper>div{
          margin: 18px 0;
          display: inline-block;
          text-align: center;
          padding: 8px 12px;
          font-size: 12px;
          line-height: 16px;
        }
        .food .body-wrapper .food-ratings .all{
          background: rgb(0,150,200);
          color: #ffffff;
        }
        .food .body-wrapper .food-ratings .good{
          margin: 0 8px !important;
          background: rgba(0,150,200,0.2);

        }
        .food .body-wrapper .food-ratings .bad{
          background: rgba(0,0,0,0.1);
        }
  .food .body-wrapper .food-ratings .ratings-body .ratings-wrapper{
    padding: 16px 0;
    border-top: 1px solid rgba(0,0,0,0.1);
  }
  .food .body-wrapper .food-ratings .ratings-body .time{
    font-size: 14px;
    color: rgb(147,153,159);
  }
  .food .body-wrapper .food-ratings .ratings-body .tall{
    font-size: 14px;
  }



</style>
