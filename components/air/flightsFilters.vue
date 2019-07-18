<template>
  <div class="filters">
    <el-row type="flex" class="filters-top" justify="space-between" align="middle">
      <el-col :span="8">
        单程：
        {{data.info.departCity}} - {{data.info.destCity}}
        /
        {{data.info.departDate}}
      </el-col>
      <el-col :span="4">
        <el-select size="mini" v-model="airport" placeholder="起飞机场" @change="handleAirport">
          <el-option
            v-for="(item,index) in data.options.airport"
            :key="index"
            :label="item"
            :value="item"
          ></el-option>
        </el-select>
      </el-col>
      <el-col :span="4">
        <el-select size="mini" v-model="flightTimes" placeholder="起飞时间" @change="handleFlightTimes">
          <el-option
            :label="`${item.from}:00 - ${item.to}:00`"
            :value="`${item.from},${item.to}`"
            v-for="(item,index) in data.options.flightTimes"
            :key="index"
          ></el-option>
        </el-select>
      </el-col>
      <el-col :span="4">
        <el-select size="mini" v-model="company" placeholder="航空公司" @change="handleCompany">
          <el-option
            :label="item"
            :value="item"
            v-for="(item,index) in data.options.company"
            :key="index"
          ></el-option>
        </el-select>
      </el-col>
      <el-col :span="4">
        <el-select size="mini" v-model="airSize" placeholder="机型" @change="handleAirSize">
          <el-option
            :label="item.name"
            :value="item.size"
            v-for="(item,index) in sizePlane"
            :key="index"
          ></el-option>
        </el-select>
      </el-col>
    </el-row>
    <div class="filter-cancel">
      筛选：
      <el-button type="primary" round plain size="mini" @click="handleFiltersCancel">撤销</el-button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: Object,
      default: {}
    }
  },
  data() {
    return {
      airport: "",
      flightTimes: "",
      company: "",
      airSize: "",

      sizePlane: [
        { name: '大', size: 'L' },
        { name: '中', size: 'M' },
        { name: '小', size: 'S' }
      ],

      // filtersList: {}
    };
  },

  methods: {
    // 选择机场时候触发
    handleAirport(value) {
        const arr = this.data.flights.filter(e =>{ 
            return e.org_airport_name === value
        })
        this.$emit('setDataList',arr)
    },

    // 选择出发时间时候触发
    handleFlightTimes(value) {
          const [ from, to ] = value.split(",")
           const arr = this.data.flights.filter(e =>{ 
            const [ start ] =e.dep_time.split(":")

            return +from < +start && +start < +to
        })
        this.$emit('setDataList',arr)
    },

    // 选择航空公司时候触发
    handleCompany(value) {
         const arr = this.data.flights.filter(e =>{ 
            return e.airline_name === value
        })
        this.$emit('setDataList',arr)
    },

    // 选择机型时候触发
    handleAirSize(value) {
         const arr = this.data.flights.filter(e =>{ 
            return e.plane_size === value
        })
        this.$emit('setDataList',arr)
    },

    // 撤销条件时候触发
    handleFiltersCancel() {}
  }
};
</script>
<style lang="less" scoped>
.filters {
  margin-bottom: 20px;
}

.filters-top {
  > div {
    /deep/ .el-select {
      margin-left: 10px;
    }
  }
}

.filter-cancel {
  margin-top: 10px;
}
</style>
</style>
