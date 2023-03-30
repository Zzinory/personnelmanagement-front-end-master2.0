<template>
  <div>
    <el-form :inline="true" ref="form" :model="leaveInfo" label-width="80px">
      <el-form-item label="请假事由">
        <el-input v-model="leaveInfo.leaveReason"></el-input>
      </el-form-item>
      <el-form-item label="请假时间">
        <el-date-picker
          v-model="leaveInfo.time"
          type="datetimerange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          align="right"
        >
        </el-date-picker>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="onSubmit">提交</el-button>
      </el-form-item>
    </el-form>
    <el-table :data="tableData" height="800" border style="width: 100%">
      <el-table-column prop="leaveReason" label="请假事由" width="180">
      </el-table-column>
      <el-table-column prop="leaveTime" label="请假时间"> </el-table-column>
      <el-table-column prop="approver" label="审批人"> </el-table-column>
      <el-table-column prop="approveStatus" label="审批状态"> </el-table-column>
    </el-table>
    <div class="Pagination">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-size="20"
        layout="total, prev, pager, next"
        :total="count"
      >
      </el-pagination>
    </div>
  </div>
</template>
<script>
import submitLeave from "../../api/submitLeave";
export default {
  data() {
    return {
      tableData: [],
      count: 0, //数据总数
      currentPage: 1, //当前页数
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now();
        },
      },

      leaveInfo: {
        time: "",
        leaveReason: "",
      },
    };
  },

  methods: {
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      this.currentPage = val;
      //调用接口分页
    },
    onSubmit() {
      let data = {
        leaveState: "待审批",
        applicantName: this.$store.state.userName,
        startTime: this.startTime,
        endTime: this.endTime,
      };
      submitLeave(data).then((resp) => {
        console.log(resp);
        alert("提交成功！");
      });
    },
    
  },
};
</script>


<style scoped>
.Pagination {
  display: flex;
  justify-content: flex-start;
  margin-top: 8px;
}
</style>
