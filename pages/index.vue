<template>
  <div class="content">
    <el-carousel :interval="3000" arrow="hover" indicator-position="none">
      <el-carousel-item v-for="(item,index) in banners" :key="index">
        <div
          class="bgcImage"
          :style="`background:url(${$axios.defaults.baseURL + item.url}) center center no-repeat; 
          background-size:contain contain`"
        ></div>
      </el-carousel-item>
    </el-carousel>

    <div class="search">
      <div class="tabBtn">
        <el-row type="flex" class="search-tab">
          <span v-for="(item,index) in btns" :key="index" @click="serverBtn(index)"
           :class="{ active: current === index}">
            <i>{{item.title}}</i>
          </span>
        </el-row>

        <el-row type="flex" class="search-input" align="middle">
          <input :placeholder="btns[current].placeholder" />
          <i class="el-icon-search"></i>
        </el-row>
      </div>
    </div>

  </div>
</template>
<script>
export default {
  data() {
    return {
      current: 0,
      banners: [],
      btns: [
        { title: "攻略", placeholder: "搜索城市" },
        { title: "酒店", placeholder: "请输入城市搜索酒店" },
        { title: "机票", placeholder: "搜索城市" }
      ]
    };
  },
  methods: {
    serverBtn(index) {
      this.current = index;
      if (index === 2) {
        this.$router.push("/air");
      }
    }
  },

  mounted() {
    this.$axios({
      url: "/scenics/banners"
    }).then(res => {
      this.banners = res.data.data;
    });
  }
};
</script>

<style lang="less" scoped>
.bgcImage {
  width: 100%;
  height: 100%;
}
.content {
  min-width: 1000px;
  margin: 0 auto;
  position: relative;

  /deep/ .el-carousel__container {
    height: 700px;
  }

  .search {
    z-index: 9;
    width: 1000px;
    position: absolute;
    left: 50%;
    top: 45%;
    margin-left: -500px;
    border-top: 1px transparent solid;

    .tabBtn {
      width: 552px;
      margin: 0 auto;
    }

    .search-tab {
      .active {
        i {
          color: #333;
        }
        &::after {
          background: #eee;
        }
      }

      span {
        width: 82px;
        height: 36px;
        display: block;
        position: relative;
        margin-right: 8px;
        cursor: pointer;

        i {
          position: absolute;
          z-index: 2;
          display: block;
          width: 100%;
          height: 100%;
          line-height: 30px;
          text-align: center;
          color: #fff;
        }

        &:after {
          position: absolute;
          left: 0;
          top: 0;
          display: block;
          content: "";
          width: 100%;
          height: 100%;
          border: 1px rgba(255, 255, 255, 0.2) solid;
          border-bottom: none;
          transform: scale(1.1, 1.3) perspective(0.7em) rotateX(2.2deg);
          transform-origin: bottom left;
          background: rgba(0, 0, 0, 0.5);
          border-radius: 1px 2px 0 0;
          box-sizing: border-box;
        }
      }
    }

    .search-input {
      width: 550px;
      height: 46px;
      background: #fff;
      border-radius: 0 4px 4px 4px;
      border: 1px rgba(255, 255, 255, 0.2) solid;
      border-top: none;
      box-sizing: unset;

      input {
        flex: 1;
        height: 20px;
        padding: 13px 15px;
        outline: none;
        border: 0;
        font-size: 16px;
      }

      .el-icon-search {
        cursor: pointer;
        font-size: 22px;
        padding: 0 10px;
        font-weight: bold;
      }
    }
  }
}
</style>
