<template>
  <div >
    <h1>{{ msg }}</h1>
    <el-table
      ref="filterTable"
      :data="tableData"
      style="width: 100%">
      <el-table-column
        prop="date"
        label="日期"
        sortable
        width="180"
        column-key="date"
        :filters="dateList"
        :filter-method="filterHandler"
      >
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="180">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址"
        :formatter="formatter">
      </el-table-column>
      <el-table-column
        prop="tag"
        label="标签"
        width="100"
        :filters="[{ text: '家', value: '家' }, { text: '公司', value: '公司' }]"
        :filter-method="filterTag"
        filter-placement="bottom-end">
        <template slot-scope="scope">
          <el-tag
            :type="scope.row.tag === '家' ? 'primary' : 'success'"
            disable-transitions>{{scope.row.tag}}</el-tag>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      msg: '配置管理',
      dateList:[],
      tableData: []
    }
  },
  mounted(){
    this.getData();
  },
   methods: {
      getData(){   //发送请求获取数据
          axios.get('http://127.0.0.1:3333/tablist')
          .then(res=>{
            console.log(res)
            if(res.data.code =='200'){
              if(res.data.result && res.data.result.length>0){
                this.tableData = res.data.result;
                for(var i=0;i<this.tableData.length;i++){
                  this.dateList.push({text: this.tableData[i].date, value: this.tableData[i].date});
                }
              }
            }
          },error=>{
            console.log(error)
          })
      },
      resetDateFilter() {
        this.$refs.filterTable.clearFilter('date');
      },
      clearFilter() {
        this.$refs.filterTable.clearFilter();
      },
      formatter(row, column) {
        return row.address;
      },
      filterTag(value, row) {
        return row.tag === value;
      },
      filterHandler(value, row, column) {
        const property = column['property'];
        return row[property] === value;
      }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
