<template>
  <div class="search-form">
    <el-row type="flex" class="search-tab">
      <span
        v-for="(item, index) in tabs"
        :key="index"
        @click="handleSearchTab(item, index)"
        :class="{active: index === currentTab}"
      >
        <i :class="item.icon"></i>
        {{item.name}}
      </span>
    </el-row>

    <el-form label-width="80px" class="search-form-content" ref="form">
      <el-form-item label="出发城市">
        <el-autocomplete
          class="el-autocomplete"
          placeholder="请搜索出发城市"
          :fetch-suggestions="queryDepartSearch"
          @select="handleDepartSelect"
          v-model="form.departCity"
        ></el-autocomplete>
      </el-form-item>

      <el-form-item label="到达城市">
        <el-autocomplete
          class="el-autocomplete"
          placeholder="请搜索到达城市"
          :fetch-suggestions="queryDestSearch"
          @select="handleDestSelect"
          v-model="form.destCity"
        ></el-autocomplete>
      </el-form-item>

      <el-form-item label="出发时间">
        <el-date-picker
          type="date"
          placeholder="选择日期"
          style="width: 100%;"
          @change="handleDate"
          v-model="form.departDate"
        ></el-date-picker>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" icon="el-icon-search" style="width: 100%" @click="handleSubmit">搜索</el-button>
      </el-form-item>

      <div class="reverse">
        <span @click="handleReverse">换</span>
      </div>
    </el-form>
  </div>
</template>

<script>
import moment from "moment";
export default {
  data() {
    return {
      form: {
        departCity: "", // 出发城市
        departCode: "", // 出发城市的代码
        destCity: "", // 到达城市
        destCode: "", // 到达城市代码
        departDate: "" // 出发日期
      },
      tabs: [
        { icon: "iconfont icondancheng", name: "单程" },
        { icon: "iconfont iconshuangxiang", name: "往返" }
      ],
      currentTab: 0
    };
  },
  methods: {
    handleSearchTab(item, index) {
      if (index === 1) {
        this.$confirm("目前不支持往返机票", "提示", {
          confirmButtonText: "确定",
          showCancelButton: false,
          type: "warning"
        });
      }
    },

    queryDepartSearch(value, cb) {
      if (!value) {
        cb([]);
        return;
      }
      this.$axios({
        url: "/airs/city",
        params: {
          name: value
        }
      }).then(res => {
        console.log(res);
        const { data } = res.data;
        const newArr = data.map(e => {
          e.value = e.name.replace("市", "");
          return e;
        });
        this.form.departCity = newArr[0].value;
        cb(newArr);
      });
    },
    handleDepartSelect(i) {
      this.form.departCity = i.value;
      this.form.departCode = i.sort;
    },

    queryDestSearch(value, cb) {
      if (!value) {
        cb([]);
        return;
      }
      this.$axios({
        url: "/airs/city",
        params: {
          name: value
        }
      }).then(res => {
        const { data } = res.data;
        const newArr = data.map(e => {
          e.value = e.name.replace("市", "");
          return e;
        });
        // console.log(newArr)
        this.form.destCity = newArr[0].value;
        cb(newArr);
      });
    },
    handleDestSelect(i) {
      console.log(i);
      this.form.destCity = i.value;
      this.form.destCode = i.sort;
    },

    handleDate(value) {
      this.form.departDate = moment(value).format("YYYY-MM-DD");
    },

    handleReverse() {
      const { departCity, departCode, destCity, destCode } = this.form;
      this.form.departCity = destCity;
      this.form.departCode = destCode;

      this.form.destCity = departCity;
      this.form.destCode = departCity;
    },

    handleSubmit() {
      const rules = {
        depart: {
          value: this.form.departCity,
          message: "请选择出发城市"
        },
        dest: {
          value: this.form.destCity,
          message: "请选目标发城市"
        },
        departDate: {
          value: this.form.departDate,
          message: "请选择出发时间"
        }
      };
      let valid = true;
      Object.keys(rules).forEach(e => {
        if (!valid) return;
        const item = rules[e];

        if (!item.value) {
          valid = false
          this.$confirm(item.message, "提示", {
            confirmButtonText: "确定",
            showCancelButton: false,
            type: "warning"
          });
        }
      });
      
      if(valid) {
        this.$router.push({
          path: "/air/flights",
          query: this.form
        });
      }
    }
  }
};
</script>

<style lang="less" scoped>
.search-form {
  border: 1px #ddd solid;
  border-top: none;
  width: 360px;
  height: 350px;
  box-sizing: border-box;
}

.search-tab {
  span {
    display: block;
    flex: 1;
    text-align: center;
    height: 48px;
    line-height: 42px;
    box-sizing: border-box;
    border-top: 3px #eee solid;
    background: #eee;
    cursor: pointer;
  }

  .active {
    border-top-color: orange;
    background: #fff;
  }

  i {
    margin-right: 5px;
    font-size: 18px;

    &:first-child {
      font-size: 16px;
    }
  }
}

.search-form-content {
  padding: 15px 50px 15px 15px;
  position: relative;

  .el-autocomplete {
    width: 100%;
  }
}

.reverse {
  position: absolute;
  top: 35px;
  right: 15px;

  &:after,
  &:before {
    display: block;
    content: "";
    position: absolute;
    left: -35px;
    width: 25px;
    height: 1px;
    background: #ccc;
  }

  &:after {
    top: 0;
  }

  &:before {
    top: 60px;
  }

  span {
    display: block;
    position: absolute;
    top: 20px;
    right: 0;
    font-size: 12px;
    background: #999;
    color: #fff;
    width: 20px;
    height: 20px;
    line-height: 18px;
    text-align: center;
    border-radius: 2px;
    cursor: pointer;

    &:after,
    &:before {
      display: block;
      content: "";
      position: absolute;
      left: 10px;
      width: 1px;
      height: 20px;
      background: #ccc;
    }

    &:after {
      top: -20px;
    }

    &:before {
      top: 20px;
    }
  }
}
</style>
