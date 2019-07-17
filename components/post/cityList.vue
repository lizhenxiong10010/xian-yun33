<template>
  <div class="citylist">
    <el-tabs type="card" tab-position="left" style="margin-top: 15px;">
      <el-tab-pane label="热门城市">
         热门城市
      </el-tab-pane>
      <el-tab-pane label="推荐城市"></el-tab-pane>
      <el-tab-pane label="奔向海岛"></el-tab-pane>
      <el-tab-pane label="主题推荐"></el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
export default {
  data() {
    return {
       
    };
  },
   
   mounted() {
        this.$axios({
             url:'/posts/cities'
        }).then(res => {
            console.log(res)
        })
   }
};
</script>
<style lang="less" scoped>
</style>

