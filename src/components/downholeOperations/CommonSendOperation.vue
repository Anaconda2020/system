<template>
  <el-dialog
    :title="sendData.wellId + '井 井下作业派工'"
    :visible.sync="sendOperVisible"
    width="60%"
    :before-close="sendOperClose"
  >
    <div class="sendOpereDiv">
      <el-form :model="dispathchInfoData" label-width="75px">
        <el-row>
          <el-col :span="6">
            <el-form-item label="上次作业:">
              <el-input v-model="sendLastData.lastOperationName" />
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="上次队伍:">
              <el-input v-model="sendLastData.lastTeamName" />
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="上次时间:">
              <el-input v-model="sendLastData.lastFinishDate" />
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="距现在:">
              <el-input v-model="sendLastData.dateCount" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="6">
            <el-form-item label="本次作业:">
              <el-input v-model="sendData.operationName" />
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="派工单号:">
              <el-input v-model="dispathchInfoData.formNum" />
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="作业队伍:">
              <el-select
                v-model="dispathchInfoData.teamId"
                placeholder="请选择"
              >
                <el-option
                  v-for="item in teamNameoptions"
                  :key="item.teamName"
                  :label="item.teamName"
                  :value="item.teamId"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="备注:">
              <el-input v-model="dispathchInfoData.remark" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item label="附件:">
          <el-upload
            action="#"
            multiple
            drag
            :show-file-list="true"
            :on-preview="handlePreview"
            :http-request="handleUpload"
            :file-list="fileLists"
            class="upload-demo"
          >
            <i class="el-icon-upload" />
            <div class="el-upload__text">
              将文件拖到此处，或<em>点击上传</em>
            </div>
          </el-upload>
        </el-form-item>
      </el-form>
    </div>
    <el-button
      type="primary"
      @click="saveSendOper(), sendOperClose()"
      class="sendOpereButton"
      >提交</el-button
    >
    <el-button type="info" @click="sendOperClose()">取消</el-button>
  </el-dialog>
</template>

<script>
export default {
  props: {
    sendOperVisible: {
      type: Boolean,
    },
    sendData: {
      type: Object,
    },
    sendLastData: {
      type: Object,
    },
  },
  inject: ["reload"],
  data() {
    return {
      teamNameoptions: [],
      dispathchInfoData: {
        dispatchId: "",
        operationNodeId: "",
        wellId: "",
        teamId: "",
        formNum: "",
        remark: "",
      },
      // 文件列表
      fileLists: [],
    };
  },
  created() {
    //初始化施工队伍信息
    this.selectTeams();
  },
  methods: {
    // 对话框父子组件传值
    sendOperClose() {
      this.$emit("sendRowClose");
    },
    // 保存修改后的信息
    saveSendOper() {
      // 将本次作业节点ID赋给派工信息对象
      this.dispathchInfoData.operationNodeId = this.sendData.operationNodeId;
      this.dispathchInfoData.wellId = this.sendData.wellId;
      this.postRequest(
        "/operation/dispatchInfo/insertDispatch",
        this.dispathchInfoData
      ).then((resp) => {
        if (resp) {
          //派工成功后更新节点状态
          this.updateOperNode(this.sendData);

          this.$message({
            message: "派工成功!",
            type: "success",
          });
          this.reload();
        } else {
          this.$message.error("派工失败，请重新提交!");
        }
      });
    },
    // 井下作业空闲施工队伍信息获取
    selectTeams() {
      this.getRequest("/operation/teamInfo/selectTeams").then((resp) => {
        if (resp) {
          console.log(resp.data);
          //将施工队伍信息赋值给下拉框
          this.teamNameoptions = resp.data;
        }
      });
    },
    //更新井下作业新节点信息
    updateOperNode(val) {
      this.putRequest("/operation/operationNode/submitOperation", val).then(
        (resp) => {
          if (resp) {
          } else {
            this.$message.error("提交失败，请重新提交!");
          }
        }
      );
    },
    // 文件上传前的判断
    handlePreview() {},
    // 上传文件
    handleUpload(params) {
      // 获取要上传的file对象
      let fileObj = params.file;
      let fd = new FormData();
      // 将文件对象添加到fd对象中
      fd.append("file", fileObj);
      console.log(fileObj);
      this.postRequest("/", fd).then((resp) => {
        if (resp) {
          this.$message({
            message: "上传成功",
            type: "success",
          });
        }
      });
    },
  },
};
</script>

<style lang="less" scoped>
.sendOpereDiv {
  height: 400px;
  overflow: auto;
}

.sendOpereDiv .el-input,
.el-select {
  width: 140px;
  height: 2px;
}

.sendOpereButton {
  margin-left: 35%;
}
</style>
