<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-list" ref="menuList" :class="{'current':currentIndex === index}" @click="selectMenu(index)">
         <span class="text" style="">
           <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
         </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="foods-list" ref="foodList">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="foods-item" >
              <div class="icon" @click="selectFood(food)">
                <img :src="food.icon" width=57px; height=57px;>
              </div>
              <div class="content" @click="selectFood(food)">
                <h2 class="name">{{food.name}}</h2>
                <p class="description">{{food.description}}</p>
                <div class="extra">
                  <span class="sellCount">月售{{food.sellCount}}份</span>
                  <span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span>¥{{food.price}}</span>
                </div>

                <div class="control-wrapper" @click.stop>
                  <control :food="food"></control>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopCart :selectFoods="selectFoods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopCart>
    <food :food="selectedFood" ref="food"></food>
  </div>


</template>

<script type="text/ecmascript-6">

  import BScroll from 'better-scroll';
  import Shopcart from 'components/shopcart/shopcart.vue';
  import Control from 'components/control/control.vue';
  import Food from 'components/food/food.vue';



  const ERR_OK = 0;

  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data(){
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood:{}
      };
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || this.scrollY >= height1 && this.scrollY < height2) {
            return i;
          }
        }
        return 0;
      },
      selectFoods(){
          let foods = [];
          this.goods.forEach((good) => {
              good.foods.forEach((food) =>{
                  if (food.count){
                      foods.push(food);
                  }
              });
          });
          return foods;
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];

      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        console.log(response);
        if (response.errno === ERR_OK) {
          this.goods = response.data;
//          console.log(this.goods);
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
//            this.firstMenu();
//            this.scrollMenu();
          });
        }
      });
    },
    methods: {
//      scrollMenu(){
//        let Cindex = this.currentIndex;
//        console.log("cc:"+Cindex);
//          if (Cindex >= 3){
//              let menuList = this.$refs.menuList;
//            this.menuScroll.scrollToElement(menuList[3],300);
//          }
//      },
//        firstMenu(){
//          let foodsList = this.$refs.foodList;
//          this.foodsScroll.scrollToElement(foodsList[2],200);
//        },
      selectMenu(index){
          let foodsList = this.$refs.foodList;
          let el = foodsList[index];
          this.foodsScroll.scrollToElement(el,300);
        },
      selectFood(food){
          this.selectedFood = food;
          this.$refs.food.show();
          //实现每次点击刷新food.vue获取评论种类问题
          this.$refs.food._findAll(food);
      },
      _initScroll(){
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
            click:true
        });
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click:true,
            probeType: 3
        });
        this.foodsScroll.on('scroll', (pos) => {
          if (pos.y <= 0) {
            this.scrollY = Math.abs(Math.round(pos.y));
          }
        });
      },
      _calculateHeight() {
        let foodList = this.$refs.foodList;
        let foodHeight = 0;
        this.listHeight.push(foodHeight);
        for (let i = 0; i < foodList.length; i++) {
          let items = foodList[i];
          foodHeight += items.clientHeight;
//          console.log("foodHeight:"+foodHeight);
          this.listHeight.push(foodHeight);
//          console.log("listHeight:"+this.listHeight);
        }
      },

    },
    components:{
      shopCart:Shopcart,
      control:Control,
      food:Food
    }
  }
</script>

<style>
  .goods {
    display: flex;
    position: absolute;
    top: 174px;
    bottom: 46px;
    width: 100%;
    overflow: hidden;
  }

  .menu-wrapper {
    /*等分 缩放 占位  */
    flex: 0 0 80px;
    /*width: 80px;*/
    background: #f5f7f9;

  }

  .goods .menu-wrapper .menu-list {
    display: table;
    padding: 0 12px;
    height: 54px;
    /*width: 80px;*/

    line-height: 14px;
    font-size: 12px;
    color: rgba(0, 0, 0, 0.6);
  }

  .menu-wrapper .current {
    background: #ffffff;
    /*position: relative;*/
    /*margin-top: -1px;*/
    /*z-index: 10; */
    /*border-bottom: none;*/
    color:red;
  }

  .menu-wrapper .text {
    display: table-cell;
    width: 56px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    vertical-align: middle;
    /*font-weight: 300;*/
  }

  .foods-wrapper {
    flex: 1;
    /*background: rgba(0,0,0,0.1);*/
  }

  .foods-list {

  }

  .foods-list .title {
    padding-left: 14px;
    line-height: 26px;
    height: 26px;
    background: #f3f5f7;
    border-left: 2px solid #d9dde1;
    font-size: 12px;
    color: rgb(147, 153, 159);
  }

  .foods-item {
    position: relative;
    display: flex;
    margin: 18px;
    padding-bottom: 18px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    font-size: 0;

  }

  .foods-item .icon {
    flex: 0 0 57px;
    width: 57px;
    height: 57px;
  }

  .foods-item .content {
    /*position: relative;*/
    flex: 0 1 auto;
    /*display: inline-block;*/
    vertical-align: top;
    margin: 2px 0 0 10px;
    font-size: 10px;
    line-height: 10px;
    color: rgb(147, 153, 159);
  }

  .foods-item .content .name {
    font-size: 14px;
    color: rgb(7, 17, 27);
    line-height: 14px;
  }

  .foods-item .content .description, .extra {
    margin-top: 8px;
    line-height: 12px;
  }

  .foods-item .content .price {
    /*display: inline-block;*/
    font-size: 14px;
    line-height: 24px;
    color: rgb(240, 20, 20);
  }

  .foods-item .content .control-wrapper{
    /*display: inline-block;*/
    position:absolute;
    right: 0;
    bottom: 12px;
  }

</style>
