<template>
  <div class="goods-lists" v-if="goods">
    <div class="goods-list">
      <li class="tips" v-for="(tip,index) in goods" :class="{'goods-sel':nowIndex==index}" @click="nowIndex=index">
        <p>{{tip.name}}</p>
      </li>
    </div>
    <!--<mystar @receive="showInfo" :msg="seller.score" :pattern="1"></mystar>-->

    <div class="pics">
      <ul v-for="good in goods">
        <div  class="tips-name">
          <h2>{{good.name}}</h2>
        </div>
        <li class="goods-food" v-for=" (lee,index2) in good.foods">
          <div class="goods-img" >
            <img :src="lee.image" width="65px" height="65px">
          </div>

          <div class="food-list">
            <h3>{{lee.name}}</h3>
            <p>{{lee.description}}</p>
            <span>月售{{lee.rating}}份  &nbsp;&nbsp;&nbsp; <b>好评率{{lee.rating}}%</b></span>
            <div class="money">
              <div class="money-lef">
                <h2 class="nowPrice">￥{{lee.price}}</h2>&nbsp;&nbsp;<b v-if="lee.oldPrice" class="oldPrice">￥{{lee.oldPrice}}</b>

              </div>


              <div class="add_list">
                <span class="icon-remove_circle_outline remNum" @click="addCart(lee.name,lee.price,buyCount,-1)"></span>
                <input class="goods-ipt" type="text" placeholder="1" v-model="buyCount">
                <span class="icon-add_circle addNum" @click="addCart(lee.name,lee.price,buyCount,1)"></span>

              </div>
            </div>

          </div>

        </li>
      </ul>
    </div>
    <div>
      <!--<my-foot @receive="showInfo" :mesg="seller.score" :pattern="1"></my-foot>-->
    </div>
  </div>
</template>

<script>

  /*  import Footer from '../footer/Footer.vue'*/
  import Bus from '../share/evenBus.js'
  export default{
    data(){
      return {
        goods:null,
        nowIndex:0,
        buyCount:1,
        num:0
      }
    },
    created(){
      this.$http.get("/api/goods").then((res)=>{
        this.goods=res.data.data;
        console.log(this.goods);
      });
    },
    methods:{
      addCart(name,price,num,way) {
        if(way>0){
          this.buyCount++;
        }else{
          this.buyCount--;
          if(this.buyCount<1){
            this.buyCount=1
          }
        }
        Bus.$emit('getTarget', {
          name:name,
          price:price,

          count:num
        });
      }
    },
    /*components: {
     myFoot:Footer

     }*/
  }
</script>

<style lang="less" rel="stylesheet/less">
  .goods-lists{
    display: flex;
    width: 100%;
    .goods-list{
      width: 80px;
      .tips{
        width: 56px;
        height: 54px;

        background: #f3f5f7;
        padding: 22px 12px 0;
        p{
          font-size: 12px;
          text-align: left;
        }
      }
      .goods-sel{
        background: #fff;
      }
    }
    .pics{
      flex: 1;
      .tips-name{
        background: #f3f5f7;
        height: 26px;
        line-height: 26px;
        font-size: 12px;
        color: rgb(147,153,159);
        border-left:3px solid #d9dde1 ;
        h2{
          margin-left: 18px;
        }
      }
      .goods-food{
        overflow: hidden;
        display: flex;
        padding: 18px;
        border-bottom: 1px solid rgba(7,17,27,0.1);

        .goods-img{
          display: inline-block;
          height: 65px;
          img{

          }
        }

        .food-list{
          flex: auto;
          display: inline-block;
          h3{
            font-size: 14px;
            padding: 4px 18px 4px 10px;
            color: rgb(7,17,27);
          }
          p{
            font-size: 10px;
            padding: 4px 18px 4px 10px;
            color: rgb(147,153,159);
          }
          span{
            font-size: 10px;
            padding: 4px 18px 4px 10px;
            color: rgb(147,153,159);
          }
          .money{
            display: flex;
            .money-lef{
              flex: auto;
              padding-left: 10px;
              .nowPrice{
                font-size: 14px;
                color: red;
                line-height: 24px;
                font-weight: 700;
                float: left;
              }
            }

            .oldPrice{
              flex: auto;
              color:rgb(147,153,159) ;
              font-size: 10px;
              line-height: 24px;
              font-weight: 700;
              text-decoration: line-through;
            }
            .add_list{
              .remNum{
                font-size: 24px;
                color: rgba(0,160,220,1);
                padding: 0;
              }
              .addNum{
                font-size: 24px;
                color: rgba(0,160,220,1);
                padding: 0;
              }
              .goods-ipt{
                width: 24px;
                font-size: 10px;
                padding-top: 3px;
                height: 20px;
                text-align: center;
                vertical-align: top;
              }
            }
          }
        }

      }

    }
  }
</style>
