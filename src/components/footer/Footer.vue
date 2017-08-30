<template>
  <div id="app">
    <div class="footer" v-if="seller">
      <div class="carbg"><span><i class="icon-shopping_cart"></i></span></div>
      <div class="footerbg">
        <span class="monery3">{{"￥"+seller.minPrice+"元起送"}}</span>
      </div>
      <div class="section">
        <span class="monery1">{{"￥"+"0"}}</span>
        <span class="monery2">{{"另需配送费￥"+seller.deliveryPrice+"元"}}</span>
       <!-- <my-message @receive="showInfo" :msg="seller.score" :pattern="1"></my-message>-->
      </div>

    </div>
  </div>
</template>

<script>
  import Bus from '../share/evenBus.js'
  export default {
    props:["mesg","pattern"],
    data(){
      return{
        seller:null,
        trolley:[]
      };
    },
    created(){
      //获取seller接口数据
      this.$http.get("/api/seller").then((res)=>{
        this.seller=res.data.data;
        console.log(this.seller);
      },(err)=>{
        console.log(err);
      });
      Bus.$on('getTarget', target => {

        console.log(target);
        this.trolley.push(target);
        console.log(this.trolley)


      });

    },
    methods:{
      showInfo:function (data) {
        this.username=data;
      },
      sendData:function () {
        console.log("sendData方法……开始发送receive事件");
        this.$emit("receive",this.name);
      }

    }
  }
</script>

<style lang="less" rel="stylesheet/less">
  .tabs {
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    .tab-item{
      flex: 1;
      text-align: center;
    }
  }
  .footer{display: flex;
    padding-left: 12px;
    position: relative;
    .carbg{
      padding-top: -6px;
      z-index: 55;
      width: 56px;
      height: 56px;
      border-radius: 50%;
      background: #141d27;
      span{
        margin: 6px;
        line-height: 50px;
        text-align: center;
        border-radius: 50%;
        display: block;
        width: 44px;
        height: 44px;
        border-radius: 50%;
        background: rgba(255,255,255,0.2);
        i{
          font-size:24px ;
          color: rgba(255,255,255,0.4);
        }
      }
    }
    .footerbg{
      position: absolute;
      top: 6px;
      padding-bottom: 8px;
      left: 0;
      width: 100%;
      height: 48px;
      z-index: -1;
      overflow: hidden;
      background:  #141d27;
      .monery3{
        position: absolute;
        right: 0px;
        height: 56px;
        line-height: 56px;
        color:rgba(255,255,255,0.4) ;
        background: rgba(255,255,255,0.1);
        flex: auto;
        text-align: center;
        display: inline-flex;
        font-size: 12px;
        padding: 0 8px;
        padding-right: 8px;
        font-weight: 700;
      }
    }
    .section{
      line-height: 24px;
      margin-top: 12px;
      margin-bottom: 12px;
      .monery1{
        padding: 0 16px 0 12px;
        margin-top: 11px;
        display: inline-flex;
        border-right: 1px solid rgba(255,255,255,0.1);
        font-size: 16px;
        color: rgba(255,255,255,0.4);
        font-weight: 700;
      }
      .monery2 {
        display: inline-flex;
        font-size: 16px;
        padding-left: 12px;
        color: rgba(255,255,255,0.4);
        font-weight: 200;
      }

    }

  }
</style>
