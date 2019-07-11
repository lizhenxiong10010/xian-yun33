<template>
  <div class="content">
    <el-row type="flex" justify="space-between" class="main">
      <div class="img">
        <nuxt-link to="/">
          <img src="http://157.122.54.189:9093/images/logo.jpg" alt />
        </nuxt-link>
      </div>
      <el-row type="flex" class="nav">
        <nuxt-link to="/">首页</nuxt-link>
        <nuxt-link to="/post">旅游攻略</nuxt-link>
        <nuxt-link to="/hotel">酒店</nuxt-link>
        <nuxt-link to="/air">国内机票</nuxt-link>
      </el-row>
      <div>
        <div class="login">
          <nuxt-link to="/user/login">登录/注册</nuxt-link>
        </div>
        <div></div>
      </div>
    </el-row>
  </div>
</template>
<script>
export default {};
</script>

<style lang="less" scoped>
.content {
  height: 60px;
  position: relative;
}
.nav {
  flex: 1;
  a {
    display: block;
    height: 60px;
    box-sizing: border-box;
    padding: 0 20px;

    &:hover {
      color: #409eff;
      border-bottom: 5px #409eff solid;
    }
  }

  .nuxt-link-exact-active {
    background-color: #409eff;
    color: #fff;
    &:hover {
      color: #fff;
    }
  }
}
.main {
  width: 1000px;
  margin: 0 auto;
  line-height: 60px;
  img {
    width: 165px;
    height: 42px;
    margin-top: 8px;
    margin-right: 15px;
  }
}
</style>
