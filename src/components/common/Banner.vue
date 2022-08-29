<template>
  <el-carousel height="720px" :interval="3000" arrow="always">
    <el-carousel-item v-for="item in images">
      <img :src=item alt="">
    </el-carousel-item>
  </el-carousel>
</template>

<script>
    export default {
        name: "Banner",

        data(){
          return{
            images: ["/static/image/banner1.jpg","/static/image/banner1.jpg","/static/image/banner1.jpg"]
          }
        }
    }
</script>

<style scoped>
  .el-carousel__item h3 {
    color: #475669;
    font-size: 18px;
    opacity: 0.75;
    line-height: 300px;
    margin: 0;
  }

  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }

  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }
</style>
