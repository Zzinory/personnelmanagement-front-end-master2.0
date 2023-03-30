<template>
  <div style="height: 100%">
    <el-container style="height: 100%">
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <!--      用户列表-->
        <el-menu
          background-color="#545c64"
          text-color="#fff"
          active-text-color="#ffd04b"
          :default-openeds="['1', '2', '3', '4']"
          style="height: 100%; border-right: none"
        >
          <el-submenu index="1">
            <template slot="title"
              ><i class="el-icon-user"></i>员工中心</template
            >
            <el-menu-item
              index="1-1"
              @click="$router.push('/home3/managerCenter')"
              >员工信息</el-menu-item
            >
            <el-menu-item
              index="1-2"
              @click="$router.push('/home3/managerSalary')"
              >员工薪资</el-menu-item
            >
          </el-submenu>
          <el-submenu index="2">
            <template slot="title"
              ><i class="el-icon-s-claim"></i>考勤管理</template
            >
            <el-menu-item
              index="2-1"
              @click="$router.push('/home3/managerAttendanceExamine')"
              >考勤情况</el-menu-item
            >
            <el-menu-item
              index="2-2"
              @click="$router.push('/home3/managerLeave')"
              >请假情况</el-menu-item
            >
            <el-menu-item
              index="2-3"
              @click="$router.push('/home3/managerOvertimeReporting')"
              >加班情况</el-menu-item
            >
            <el-menu-item
              index="2-4"
              @click="$router.push('/home3/managerAwayOfficialReporting')"
              >出差情况</el-menu-item
            >
            <el-menu-item
              index="2-5"
              @click="$router.push('/home3/managerAttendanceSummary')"
              >考勤总结</el-menu-item
            >
          </el-submenu>

          <el-submenu index="3">
            <template slot="title"
              ><i class="el-icon-date"></i>公司事务</template
            >

            <el-menu-item
              index="3-1"
              @click="$router.push('/home3/managerAnnouncementPlan')"
              >信息公告</el-menu-item
            >
            <el-menu-item
              index="3-2"
              @click="$router.push('/home3/managerPlanPlan')"
              >活动安排</el-menu-item
            >
          </el-submenu>
        </el-menu>
      </el-aside>

      <el-container class="home2-container">
        <!-- <el-header style="background: #409EFF;border-radius: 8px;color: white">人事管理系统</el-header> -->
        <div class="header-container">
          <el-breadcrumb separator="/">
            <el-breadcrumb-item :to="{ path: '/home3' }"
              >首页</el-breadcrumb-item
            >
            <el-breadcrumb-item
              v-for="(item, index) in $route.meta"
              :key="index"
              >{{ item }}</el-breadcrumb-item
            >
          </el-breadcrumb>
          <el-dropdown @command="handleCommand" menu-align="start">
            <img src="https://gw.alipayobjects.com/zos/rmsportal/BiazfanxmamNRoxxVxka.png" class="avator" />
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item command="home">首页</el-dropdown-item>
              <el-dropdown-item command="signout">退出</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </div>
        <el-main><router-view></router-view></el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  name: "home2",
  mounted() {
    console.log(123, this.$route);
  },
  methods: {
    async handleCommand(command) {
      if (command == "home") {
        this.$router.push("/home3/staffCenter");
      } else if (command == "signout") {
        //signout 待补充
        // const res = await signout()
        // if (res.status == 1) {
        // 	this.$message({
        //                 type: 'success',
        //                 message: '退出成功'
        //             });
        //             this.$router.push('/');
        // }else{
        // 	this.$message({
        //                 type: 'error',
        //                 message: res.message
        //             });
        // }
      }
    },
  },
};
</script>

<style scoped>
.home2-container {
  flex-direction: column;
}
	.header-container{
		background-color: #EFF2F7;
		height: 60px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding-left: 20px;
	}

.el-breadcrumb {
  background-color: #eff2f7;
  height: 60px;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-pack: justify;
  -ms-flex-align: center;
  align-items: center;
  padding-left: 20px;
}
.avator {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  margin-right: 37px;
}
.el-dropdown-menu__item {
  text-align: center;
}

.el-header,
.el-footer {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
  line-height: 60px;
}

.el-aside {
  background-color: #d3dce6;
  color: #333;
  line-height: 200px;
  margin-left: 0px;
}

.el-main {
  background-color: #fff;
  color: #333;
  /* text-align: center; */
  /* line-height: 160px; */
}

body,
html {
  height: 100%;
  margin: 0;
}
body > .el-container {
  margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
  line-height: 320px;
}
</style>
