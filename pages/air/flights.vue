<template>
  <section class="contianer">
    <el-row type="flex" justify="space-between">
      <!-- 顶部过滤列表 -->
      <div class="flights-content">
        <!-- 过滤条件 -->
        <div>
          <FlightsFilters :data="options" @airName="arr" />
        </div>

        <!-- 航班头部布局 -->
        <div>
          <FlightsListHead />
        </div>

        <!-- 航班信息 -->
        <div>
          <FlightsItem v-for="(item, index) in dataList" :key="index" :data="item" />
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="pageIndex"
            :page-sizes="[5, 10, 20, 100]"
            :page-size="pageSize"
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
import moment from "moment";
import FlightsListHead from "@/components/air/flightsListHead.vue";
import FlightsItem from "@/components/air/flightsItem.vue";
import FlightsFilters from "@/components/air/flightsFilters.vue";
export default {
  data() {
    return {
      options: {
        data: {
          info: {},
          options: {}
        }
      },
      SDataList: [],
      dataList: [],
      pageIndex: 1,
      total: 0,
      pageSize: 5
    };
  },
  methods: {
    arr(arr){
      this.dataList=arr
      this.total=arr.length
      // console.log(arr);
    },
    ShouDataList() {
      const start = (this.pageIndex - 1) * this.pageSize;
      const end = start + this.pageSize;
      this.dataList = this.SDataList.slice(start, end);
    },
    handleSizeChange(val) {
      this.pageIndex = 1;
      this.pageSize = val;
      this.ShouDataList();
    },
    handleCurrentChange(val) {
      this.pageIndex = val;
      this.ShouDataList();
    }
  },
  components: {
    FlightsListHead,
    FlightsItem,
    FlightsFilters
  },
  mounted() {
    this.$axios({
      url: "airs",
      params: this.$route.query
    }).then(res => {
      this.options = res;
      this.dataList = res.data.flights;
      this.SDataList = res.data.flights;
      console.log(res, 456);
      console.log(this);

      console.log(this.options, 123);
      this.total = res.data.total;
      this.ShouDataList();
    });
  },
  props: ["data"]
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