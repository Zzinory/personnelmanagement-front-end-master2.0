<template>
  <div>
  
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
        :filters="[{text: '2016-05-01', value: '2016-05-01'}, {text: '2016-05-02', value: '2016-05-02'}, {text: '2016-05-03', value: '2016-05-03'}, {text: '2016-05-04', value: '2016-05-04'}]"
        :filter-method="filterHandler"
      >
      </el-table-column>
      <el-table-column
        prop="title"
        label="标题"
        >
      </el-table-column>
      <el-table-column
        prop="op"
        label="查看详情"
        width="180">
         <template slot-scope="scope">
           <el-button @click="handleDetail(scope.row)">查看详情</el-button>
        </template>
      </el-table-column>
     
    </el-table>
     <el-dialog :title="announcement.title" :visible.sync="dialogFormVisible">
       <div>{{announcement.desc}}</div>
    </el-dialog>
  </div>

</template>

<script>
export default {
  data() {
    return {
      dialogFormVisible: false,
      announcement:{},
      tableData: [{
        date: '2016-05-02',
        title: '公告1',
        desc:'公告详情111',
        
      }, {
        date: '2016-05-04',
       title: '公告2',
        desc:'公告详情222',
      }, {
        date: '2016-05-01',
        title: '公告3',
         desc:'公告详情333',
      }, {
        date: '2016-05-03',
        title: '公告4',
         desc:'公告详情444',
      }]
    }
  },
  methods: {
    handleDetail(row){
      this.dialogFormVisible = true;
      this.announcement = row
      
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

<style scoped>

</style>
