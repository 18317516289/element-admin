<template>
  <div>
    <el-dialog :visible.sync="visible" width="600px">
      <h4>修改用户</h4>

      <el-form :inline="true" label-width="180px">
        <el-form-item label="用户名（账号）：">
          <el-input v-model="model.name" type="text" placeholder="姓名"></el-input>
        </el-form-item>

        <el-form-item label="手机号：">
          <el-input type="text" v-model="model.phone"></el-input>
        </el-form-item>

        <el-form-item label="密码：" prop="trueContext">
          <el-input type="text" v-model="model.password"></el-input>
        </el-form-item>
        <el-form-item label="性别：" prop="Sexs" width="120px">
          <el-select v-model="model.sex" placeholder="请选择">
            <el-option
              v-for="item in Sexs"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="角色：" prop="Roles">
          <el-select v-model="model.roleId" placeholder="请选择">
            <el-option
              v-for="item in Roles"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>

        <div style=" margin-left: 252px;margin-top:25px">
          <el-button type="primary" @click="remove()">重置</el-button>
          <el-button type="primary" style="margin-left:5px" @click="onReturn()">返回</el-button>
          <el-button
            type="primary"
            style="margin-left:5px"
            @click="onUpdate()"
            :loading="updateLoading"
          >确认</el-button>
        </div>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      model: {},
      visible: false,
      Sexs: [
        {
          value: 0,
          label: "男"
        },
        {
          value: 1,
          label: "女"
        }
      ],
      Roles: [
        {
          value: 0,
          label: "普通用户"
        },
        {
          value: 1,
          label: "管理员"
        }
      ],
      updateLoading: false,
      show(row) {
          debugger
        this.model = row;
        this.visible = true;
      }
    };
  },
  methods: {
    remove() {
      const self = this;
      self.model = {};
    },
    onReturn() {
      this.model = {};
      this.visible = false;
      this.$emit("submit");
    },
    onUpdate() {
      const self = this;
      self.updateLoading = true;
      self
        .$axios({
          method: "put",
          url: "/User",
          data: {
            ...self.model
          }
        })
        .then(e => {
          self.addLoading = false;
          self.visible = false;
          self.$emit("submit");
        })
        .catch(e => {
          self.addLoading = false;
          self.$message.error(e);
        });
    }
  }
};
</script>

<style>
</style>