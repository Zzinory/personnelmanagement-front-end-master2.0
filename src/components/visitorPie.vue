<template>
  <div class="visitorpie">
    <div id="visitorpie" class="" style="width: 90%; height: 450px"></div>
  </div>
</template>

<script>
import * as echarts from "echarts/lib/echarts.js";
import "echarts/lib/chart/pie";
import "echarts/lib/component/title";
import "echarts/lib/component/legend";
import { TooltipComponent } from "echarts/components";
echarts.use([TooltipComponent]);

export default {
  mounted() {
    this.myChart = echarts.init(document.getElementById("visitorpie"));
    this.initData();
  },
  props: ["pieData"],
  methods: {
    initData() {
      const option = {
        title: {
          text: "考勤总结",
          subtext: "",
          x: "center",
        },

        legend: {
          orient: "vertical",
          left: "left",
          data: ["请假", "加班", "出勤", "出差"],
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: "55%",
            center: ["50%", "60%"],
            data: [
              { value: this.pieData.leaveCount, name: "请假" },
              { value: this.pieData.overTimeCount, name: "加班" },
              { value: this.pieData.attendanceCount, name: "出勤" },
              { value: this.pieData.awayOfficialCount, name: "出差" },
            ],
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)",
              },
            },
          },
        ],
      };

      this.myChart.setOption(option);
    },
  },
  watch: {
    pieData: function () {
      this.initData();
    },
  },
};
</script>

<style scoped>
.visitorpie {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
</style>
