<template>
  <section class="contianer">
    <el-row type="flex" justify="space-between">
      <!-- 顶部过滤列表 -->
      <div class="flights-content">
        <!-- 过滤条件 -->
        <div>
          <FlightsFilters :data="cacheFlightsData" @setDataList="setDataList"/>
        </div>

        <!-- 航班头部布局 -->
        <div>
          <FlightsListHead />
        </div>

        <!-- 航班信息 -->
        <div>
          <FlightsItem v-for="(item,index) in dataList" :key="index" :data="item" />
        </div>

        <!-- 分页 -->
        <div>
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="pagenum"
            :page-sizes="[2, 4, 6, 8]"
            :page-size="pagesize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
          ></el-pagination>
        </div>
      </div>

      <!-- 侧边栏 -->
      <div class="aside">
        <!-- 侧边栏组件 -->
      </div>
    </el-row>
  </section>
</template>

<script>
import FlightsListHead from "@/components/air/flightsListHead";
import FlightsItem from "@/components/air/flightsItem";
import FlightsFilters from "@/components/air/flightsFilters";

export default {
  components: {
    FlightsListHead,
    FlightsItem,
    FlightsFilters
  },
  data() {
    return {
      cacheFlightsData: {
        flights: [],
        info: {},
        options: {}
      },
      flightsList: {
        flights: [],
        info: {},
        options: {}
      },
      
      pagenum: 1,
      pagesize: 8,
      total:0
    };
  },

  computed: {
      dataList(){
       return this.flightsList.flights.slice(
           (this.pagenum - 1) * this.pagesize,
           this.pagenum * this.pagesize
      )
  }
},

  methods: {
    getData() {
      this.$axios({
        url: "/airs",
        params: this.$route.query
      }).then(res => {
        console.log(res);
        this.flightsList = res.data;
        this.cacheFlightsData = {...res.data}
        // this.total = res.data.total;

      });
    },
    handleSizeChange(val) {
      //  console.log(`每页 ${val} 条`);
      this.pagesize = val;
      this.pagenum = 1;
    
    },

    handleCurrentChange(val) {
      //  console.log(`当前页: ${val}`);
      this.pagenum = val;
    },
    setDataList( arr ){
            this.flightsList.flights = arr;
        },
  

  },

  mounted() {
    this.getData();
  }
};
</script>

<style scoped lang="less">
.contianer {
  width: 1000px;
  margin: 20px auto;
}

.flights-content {
  width: 745px;
  font-size: 14px;
}

.aside {
  width: 240px;
}
</style>