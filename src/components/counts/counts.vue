<template>
    <div>

      <div class="add_list">
        <span class="icon-remove_circle_outline remNum" @click="addCart(mesg,gprice,buyCount,-1)"></span>
        <input class="goods-ipt" type="text" placeholder="1" v-model="buyCount">
        <span class="icon-add_circle addNum" @click="addCart(mesg,gprice,buyCount,1)"></span>

      </div>
    </div>
</template>

<script>
  import Bus from '../share/evenBus.js'
    export default{
      props:["mesg","gprice"],
      data(){
        return{
          seller:null,
          trolley:[],
          buyCount:1,
        };
      },
      created(){
        //获取seller接口数据
        this.$http.get("/api/seller").then((res)=>{
          this.seller=res.data.data;
          /*console.log(this.seller);*/
          console.log("sssssssssssssssss")
        },(err)=>{
          console.log(err);
        });
        Bus.$on('getTarget', target => {

          /*console.log(target);*/
          this.trolley.push(target);
          /*console.log(this.trolley)*/


        });

      },
      methods:{
        addCart(name,price2,num,way) {
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
            price:price2,
            count:num
          });
        }
      }
    }
</script>

<style lang="less" rel="stylesheet/less">
  .add_list {

  .remNum {
    font-size: 24px;
    color: rgba(0, 160, 220, 1);
    padding: 0;
  }

  .addNum {
    font-size: 24px;
    color: rgba(0, 160, 220, 1);
    padding: 0;
  }

  .goods-ipt {
    width: 24px;
    font-size: 10px;
    padding-top: 3px;
    height: 20px;
    text-align: center;
    vertical-align: top;
  }

  }
</style>
