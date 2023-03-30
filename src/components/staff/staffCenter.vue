<template>
  <div class="staff-center-wrapper">
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <div class="header-left">
          <span style="margin-left: 10px">员工基本信息</span>
        </div>
        <el-button
          style="float: right; padding: 3px 0"
          type="text"
          @click="handleEdit"
          >编辑</el-button
        >
      </div>

      <div class="card-wrapper">
        <div v-for="o in fieldMap" :key="o.renderKey" class="col-wrapper">
          <div class="col-item">
            <div class="col-left">{{ o.renderKey }}:</div>
            <div class="col-right">{{ form[o.renderName] }}</div>
          </div>
        </div>
      </div>
    </el-card>

    <el-upload
      class="avatar-uploader"
      action="https://jsonplaceholder.typicode.com/posts/"
      :show-file-list="false"
      :on-success="handleAvatarSuccess"
      :before-upload="beforeAvatarUpload"
    >
      <img v-if="imageUrl" :src="imageUrl" class="avatar" />
      <i v-else class="el-icon-plus avatar-uploader-icon"></i>
    </el-upload>

  

    <el-dialog title="修改信息" :visible.sync="dialogFormVisible">
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="工号">
          <el-input v-model="form.id" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="姓名">
          <el-input v-model="form.userName"></el-input>
        </el-form-item>
        <el-form-item label="账户">
          <el-input v-model="form.userAccount"></el-input>
        </el-form-item>
        <el-form-item label="密码">
          <el-input v-model="form.password" show-password></el-input>
        </el-form-item>
        <el-form-item label="职位">
          <el-input v-model="form.position" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="入职日期">
          <el-input v-model="form.entryDate"></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="onSubmit">修改</el-button>
          <el-button type="primary">确定</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>
<script>
import getUser from "../../api/getUser";
import modifyUser from "../../api/modifyUser";
export default {
  data() {
    return {
      dialogFormVisible: false,
      imageUrl: 'https://gw.alipayobjects.com/zos/rmsportal/BiazfanxmamNRoxxVxka.png',
      form: {
        id: "1",
        userName: "测试",
        userAccount: "123",
        password: "123**",
        position: "总裁",
        entryDate: "2022-1-1",
      },
      fieldMap: [
        { renderKey: "工号", renderName: "id" },
        { renderKey: "姓名", renderName: "userName" },
        { renderKey: "账户", renderName: "userAccount" },
        { renderKey: "密码", renderName: "password" },
        { renderKey: "职位", renderName: "position" },
        { renderKey: "入职日期", renderName: "entryDate" },
      ],
    };
  },
  methods: {
    handleAvatarSuccess(res, file) {
      this.imageUrl = URL.createObjectURL(file.raw);
    },
    beforeAvatarUpload(file) {
      const isJPG = file.type === "image/jpeg";
      const isLt2M = file.size / 1024 / 1024 < 2;

      if (!isJPG) {
        this.$message.error("上传头像图片只能是 JPG 格式!");
      }
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 2MB!");
      }
      return isJPG && isLt2M;
    },
    handleEdit() {
      this.dialogFormVisible = true;
    },

    onSubmit() {
      let user = {
        id: this.form.id,
        userName: this.form.userName,
        userAccount: this.form.userAccount,
        password: this.form.password,
      };
      modifyUser(user).then((resp) => {
        // console.log(resp.data);
        if (resp.data.code == 0) {
          alert("修改成功");
          this.$store.commit("setAccount", user.userAccount);
          this.$store.commit("setUserName", resp.data.data.userName);
        } else {
          if (resp.data.code == 1) alert("修改失败，账户名重复");
        }
      });
    },
  },
  mounted() {
    console.log(123, this.$route);
  },
  created() {
    let user = { userAccount: this.$store.state.account };
    getUser(user).then((resp) => {
      console.log(resp.data.data);
      if (resp.data.data.role == 1) resp.data.data.position = "管理员";
      if (resp.data.data.role == 2) resp.data.data.position = "普通员工";
      if (resp.data.data.role == 1) resp.data.data.position = "人事管理";
      this.form = resp.data.data;
    });
    //
    // console.log(this.$store.state.account);
    // this.form.account=this.$store.state.account;
  },
};
</script>


<style scoped>
.staff-center-wrapper {
  display: flex;
  justify-content: space-around;
}
.col-wrapper {
  margin-bottom: 18px;
  font-size: 14px;
  display: flex;
  flex-wrap: wrap;
  width: 300px;
}
.col-left {
  margin-right: 10px;
}
.header-left {
  display: flex;
  align-items: center;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both;
}

.box-card {
  width: 800px;
}
.col-item {
  display: flex;
  width: 100%;
}
.avatar-uploader ::after{
  content: '点击上传头像';
  font-size: 12px;
}
.card-wrapper {
  display: flex;
  flex-wrap: wrap;
}
.el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>
