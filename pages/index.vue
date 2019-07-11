<template>
  <div class="content">
    <el-carousel :interval="3000" arrow="hover">
      <el-carousel-item v-for="(item,index) in slideshow" :key="index">
        <div class="small">{{ item }}</div>
      </el-carousel-item>
    </el-carousel>
  </div>
</template>
<script>
export default {
  data() {
    return {
      slideshow: []
    };
  },

  mounted() {
      this.$axios({
          url:`/scenics/banners`,
      }).then(res => {
         console.log(res)
      })
  }
};
</script>

<style lang="less" scoped>
</style>
