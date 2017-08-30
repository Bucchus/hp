<template>
  <div v-if="goods">
    <p v-for="good in goods[1].foods">

      <img :src="good.image" width="64px">

    </p>
  </div>
</template>

<script>
  export default{
    data(){
      return {
        goods:null,
        good:null
      }
    },
    created(){
      this.$http.get("/api/goods").then((res) => {
        console.log(res);
        this.goods=res.data.data;

        console.log(this.goods[0]);
      },(err) => {
        console.log(err)
      })
    }
  }
</script>

<style>
</style>
