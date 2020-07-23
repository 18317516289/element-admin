<template>
  <div>
    <index1 :topicType="0" :childMethods="childMethods"></index1>
    <index2 :topicType="1" :childMethods="childMethods"></index2>
    <el-form :inline="true">
      <div style=" margin-left: 252px;margin-top:25px">
        <el-button
          type="primary"
          style="margin-left:5px"
          @click="OnSubmit()"
          :loading="addLoading"
        >确认提交</el-button>
      </div>
    </el-form>
  </div>
</template>

<script>
import index1 from "./index";
import index2 from "./index2";
export default {
  components: { index1, index2 },
  data() {
    return {
      datatabelAll: [],
      datatabel1: [],
      datatabel2: [],
      addLoading: false,
      childMethods: {
        onChange1: this.onChange1,
        onChange2: this.onChange2
      }
    };
  },
  methods: {
    onChange1(item) {
      debugger;
      this.datatabel1 = item;
    },
    onChange2(item) {
      this.datatabel2 = item;
    },
    OnSubmit() {
      const self = this;
      self.addLoading = true;
      self.datatabelAll = []
      for (const item1 of self.datatabel1) {
        self.datatabelAll.push(item1);
      }
      for (const item2 of self.datatabel2) {
        self.datatabelAll.push(item2);
      }
      self
        .$axios({
          method: "post",
          url: "TestQuestions",
          data: { Tests: self.datatabelAll }
        })
        .then(e => {
          debugger;
          if (e.data.success) {
            self.addLoading = false;
            self.$message.success("成功");
          } else {
            self.addLoading = false;
          }
        })
        .catch(e => {
          self.addLoading = false;
        });
    }
  },
  created() {}
};
</script>

<style>
</style>