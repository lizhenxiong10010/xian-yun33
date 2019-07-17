<template>
  <section class="container">
      <el-row type="flex" justify="space-between">
      <!-- 搜索表单 -->
       <CityList />

      <!-- banner广告 -->
      <div class="sale-banner">
        
      </div>
    </el-row>
         
   </section>
</template>
<script>
import CityList from "@/components/post/cityList"
export default {
    components: {
       CityList
    }
}
</script>

<style lang="less" scoped>
.container{
    width: 1000px;
    margin: 0 auto
}
</style>