<template>
  <div>
    <el-dialog :visible.sync="visible" width="600px">
      <h4>添加试题</h4>
      <el-form :inline="true" label-width="220px">
        <el-form-item label="题目" prop="tittle">
          <el-input v-model="model.tittle" type="textarea" placeholder="题目"></el-input>
        </el-form-item>
        <el-form-item label="试题类型">
          <el-select v-model="model.topicType">
            <el-option
              v-for="item in options"
              :disabled="model.topicType>=0"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>

        <span v-if="model.topicType==0">
          <el-form-item label="正确答案" prop="winSelectResult">
            <el-input type="text" placeholder="正确答案" v-model="model.winSelectResult"></el-input>
          </el-form-item>

          <el-form-item label="答案A：" prop="selectA">
            <el-input type="textarea" v-model="model.selectA"></el-input>
          </el-form-item>
          <el-form-item label="答案B：" prop="selectB">
            <el-input type="textarea" v-model="model.selectB"></el-input>
          </el-form-item>
          <el-form-item label="答案C：" prop="selectC">
            <el-input type="textarea" v-model="model.selectC"></el-input>
          </el-form-item>
          <el-form-item label="答案D：" prop="selectD">
            <el-input type="textarea" v-model="model.selectD"></el-input>
          </el-form-item>
        </span>
        <span v-else>
          <el-form-item label="正确答案" v-model="model.winIsTrueResult" prop="winIsTrueResult">
            <el-input type="text" placeholder="正确答案" :value="model.winIsTrueResult|result"></el-input>
          </el-form-item>

          <el-form-item label="正确：" prop="trueContext">
            <el-input type="textarea" v-model="model.trueContext"></el-input>
          </el-form-item>
          <el-form-item label="错误：" prop="falseContext">
            <el-input type="textarea" v-model="model.falseContext"></el-input>
          </el-form-item>
        </span>
        <div style=" margin-left: 252px;">
          <el-button type="primary" @click="remove()">重置</el-button>
          <el-button type="primary" style="margin-left:5px" @click="onReturn()">返回</el-button>
          <el-button
            type="primary"
            style="margin-left:5px"
            @click="onAdd()"
            :loading="addLoading"
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
      options: [
        {
          value: 0,
          label: "选择题"
        },
        {
          value: 1,
          label: "判断题"
        }
      ],
      visible: false,
      upLoading: false,
      model: {},
      addLoading: false,
      show(type) {
        this.visible = true;
        this.model.topicType = type;
      }
    };
  },
  methods: {
    remove() {
      const self = this;
      let type = self.model.topicType
      self.model = {};
      self.model.topicType = type
    },
    onReturn() {
      this.model = {};
      this.visible = false
      this.$emit("submit");
    },
    onAdd() {
      const self = this;
      self.addLoading = true;
      self
        .$axios({
          method: "post",
          url: "/TopicInfo",
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
  },
  filters: {
    result(str) {
      if (str == true) return "正确";
      else {
        return "错误";
      }
    }
  }
};
</script>

<style>
</style>