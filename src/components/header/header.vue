<template>
  <div class="header">
    <div class="detailbbbb"></div>
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}</span>
        <i class="icon-thumb_up"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-thumb_up"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" >
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item,index) in seller.supports">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-remove_circle_outline"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from 'components/star/star.vue';

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail() {
        this.detailShow = true;

      },
      hideDetail() {
        this.detailShow = false;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      star
    }
  };
</script>

<style>
  @import "../../common/stylus/style.css";

  body, html {
    line-height: 1;
    font-weight: 200;
    font-family: 'PingFang SC', 'STHeitiSC-Light', 'Helvetica-Light', arial, sans-serif;
  }

  .header{
    position: relative;
    overflow: hidden;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.2);
    /*filter: blur(10px);*/
  }

    .content-wrapper{
      position: relative;
      padding: 24px 0 18px 24px;
      font-size: 0;
  }
      .avatar{
        border-radius: 2px;
        display: inline-block;
      }
      .content-wrapper .content{
        display: inline-block;
        margin-left: 16px;
        margin-top: 2px;
        vertical-align: top;
        font-size: 0px;
        color: #ffffff;
      }
        .content-wrapper .title{}
          .title .brand{
          vertical-align: top;
          display: inline-block;
          width: 30px;
          height: 18px;
          background-size: 30px 18px;
          background-repeat: no-repeat;
          background-image: url("brand@2x.png");
        }
          .title .name{
          margin-left: 6px;
          line-height: 18px;
          font-size: 18px;
          font-weight: bold;
        }
        .content-wrapper .description{
          margin: 8px 0 10px 0;
          line-height: 12px;
          font-size: 12px;
          font-weight: 300;

        }
        .content-wrapper .support{
          margin-bottom: 2px;
        }
          .content-wrapper .support .icon{
            display: inline-block;
            width: 12px;
            height: 12px;
            background-size: 12px 12px;
            background-repeat: no-repeat;
            background-image: url("decrease_1@2x.png");
          }
          .content-wrapper .support .text{
            margin-left: 4px;
            vertical-align: top;
            font-size: 10px;
            line-height: 12px;
            font-weight: 300;

          }
      .support-count{
        display: inline-block;
        position: absolute;
        right: 12px;
        bottom: 14px;
        padding: 0px 8px;
        height: 24px;
        line-height: 24px;
        background-color: rgba(0,0,0,0.2);
        border-radius: 10px;
        color: #ffffff;
        text-align: center

      }
          .count{
            font-size: 10px;
            font-weight: 300;
            vertical-align: top;
            /*margin-right: 2px;*/
          }
          .icon-thumb_up{
            margin-left: 2px;
            line-height: 24px;
            font-size: 10px;
          }

    .bulletin-wrapper{
      position: relative;
      height: 28px;
      line-height: 28px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      color: #ffffff;
      background-color: rgba(0,0,0,0.2);
      }
      .bulletin-title{
        display: inline-block;
        vertical-align: top;
        margin-left: 12px;
        margin-top: 8px;
        width: 22px;
        height: 12px;
        background-size: 22px 12px;
        background-repeat: no-repeat;
        background-image: url("bulletin@2x.png");
      }
      .bulletin-text{
        /*position: relative;*/
        vertical-align: top;
        /*display: inline-block;*/
        font-size: 10px;
        font-weight: 200;
        line-height: 28px;
        margin: 0 4px;

      }
      .bulletin-wrapper  .icon-thumb_up{
        position: absolute;
        display: inline-block;
        right: 0px;
        top: 4px;
        font-size: 10px;
        /*color: black;*/
        margin-left: 14px;
      }

    .background{
    position: absolute;
    left: 0;
    top: 0;
    z-index: -1;
    width: 100%;
    height: 100%;
    filter: blur(10px);
  }
      .background img{  width: 100%;  height: 100%;  }


  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.2s;
  }
  .fade-enter, .fade-leave-to  {
    opacity: 0;
  }
    .detail{
      position: relative;
      position: fixed;
      top:0;
      left: 0;
      z-index: 100;
      overflow: auto;
      width:100%;
      height:100%;
      background-color: rgba(7,17,27,0.9);
      /*-webkit-backdrop-filter: blur(10px);*/
      font-weight: 300;
      line-height: 12px;
    }
    .detail .detail-wrapper{
        padding: 64px 36px 32px 36px;
        width: 80%;
        color: white;
        min-height: 100%;
        margin: 0 0;
      }
        .detail-main{
          /*text-align: center;*/
        }
          .detail-main .name{
            text-align: center;
            font-size: 16px;
            font-weight: 700;
            line-height: 16px;
        }
          .detail-main .title{
            display: flex;

          }
            .detail-main .title .line{
              flex: 1;
              position: relative;
              top: -6px;
              border-bottom: 1px solid rgba(255,255,255,0.2);
            }
            .detail-main .title .text{
              text-align: center;
              font-size: 14px;
              font-weight: 700;
              line-height: 14px;
              padding: 0 12px;
            }
            .detail-main .supports{
              /*background-color: #42b983;*/
              margin: 24px 12px 28px 12px;
            }
              .support-item{
                font-size: 0;
                margin-bottom: 12px;
              }
                .support-item .icon{
                  display: inline-block;
                  position: relative;
                  vertical-align: top;
                  top: -2px;
                  margin: 0 6px 0 0;
                  width: 16px;
                  height: 16px;
                  border-radius: 2px;
                  background-size: 16px 16px;
                  background-repeat: no-repeat;

                }
                .support-item .decrease{background-image: url("decrease_2@2x.png")}
                .support-item .discount{background-image: url("discount_2@2x.png")}
                .support-item .guarantee{background-image: url("guarantee_2@2x.png")}
                .support-item .invoice{background-image: url("invoice_2@2x.png")}
                .support-item .special{background-image: url("special_2@2x.png")}
                .support-item .text{
                  font-weight: 300;
                  font-size: 12px;
                }

          .bulletin{
            margin: 24px 12px 72px 12px;
          }
            .bulletin .content{
              line-height: 24px;
              font-size: 12px;
              font-weight: 300;
            }

      .detail-close{
        text-align: center;
        margin-top: -64px;
        font-size: 32px;
        color: rgba(255,255,255,0.5);
      }

  .star-wrapper{
    margin-top: 16px;
    margin-bottom: 28px;
    padding: 2px 0;
    text-align: center;
  }







</style>
