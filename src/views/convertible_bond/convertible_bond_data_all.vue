<template>
  <div class="app-container">
    <el-table
      :data="list"
      border
      fit
      highlight-current-row
    >

      <el-table-column align="center" label="序号" width="95">
        <template slot-scope="scope">
          {{ scope.$index + 1 }}
        </template>
      </el-table-column>
      <el-table-column label="可转债代码">
        <template slot-scope="scope">
          {{ scope.row.secucode }}
        </template>
      </el-table-column>
      <el-table-column label="可转债名称">
        <template slot-scope="scope">
          {{ scope.row.security_NAME_ABBR }}
        </template>
      </el-table-column>
      <el-table-column label="发行日期">
        <template slot-scope="scope">
          {{ scope.row.security_START_DATE | formatToStdDate}}
        </template>
      </el-table-column>
      <el-table-column label="正股代码">
        <template slot-scope="scope">
          {{ scope.row.security_NAME_ABBR }}
        </template>
      </el-table-column>
      <el-table-column label="正股名称">
        <template slot-scope="scope">
          {{ scope.row.security_NAME_ABBR }}
        </template>
      </el-table-column>
      <el-table-column label="正股价格">
        <template slot-scope="scope">
          {{ scope.row.convert_STOCK_PRICE }}
        </template>
      </el-table-column>
      <el-table-column label="转股价格">
        <template slot-scope="scope">
          {{ scope.row.transfer_PRICE }}
        </template>
      </el-table-column>
      <el-table-column label="转股价值">
        <template slot-scope="scope">
          {{ scope.row.transfer_VALUE }}
        </template>
      </el-table-column>
      <el-table-column label="债现价">
        <template slot-scope="scope">
          {{ scope.row.current_BOND_PRICE }}
        </template>
      </el-table-column>
      <el-table-column label="转股溢价率">
        <template slot-scope="scope">
          {{ scope.row.transfer_PREMIUM_RATIO }}
        </template>
      </el-table-column>
      <el-table-column label="转股开始日期">
        <template slot-scope="scope">
          {{ scope.row.transfer_START_DATE | formatToStdDate }}
        </template>
      </el-table-column>
      <el-table-column label="转股结束日期">
        <template slot-scope="scope">
          {{ scope.row.transfer_END_DATE | formatToStdDate}}
        </template>
      </el-table-column>
      <el-table-column label="数据刷新时间">
        <template slot-scope="scope">
          {{ scope.row.insert_date | formatToStdDate}}
        </template>
      </el-table-column>


    </el-table>

  </div>
</template>

<script>

const axios = require('axios')

export default {

  data() {
    return {
      list: [],

    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    async fetchData() {
      var currDate = this.getCurrDate();
      const {data: res} = await
        this.$http.get(`/convertible_bond/listLastDayConvertibleBondData?searchDate=${currDate}`);
      console.log(res);
      if (res.code === 0) {
        console.log(res.data)
        this.list = res.data;
      } else {
        console.error("get error ")
      }
    },
    /** 获取当期的日期 */
    getCurrDate() {
      var time = new Date(+new Date() + 8 * 3600 * 1000).toJSON().substr(0, 19).replace("T", " ");
      return time.toString().substr(0, 10)
    },

  },
  filters:{

    // 截取日期时间的前10位，
    formatToStdDate(dateStr){
        return dateStr.substr(0,10);
    }
  }
}
</script>
