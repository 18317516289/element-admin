<template>
  <div>
    <div style="margin-left: 800px;font-size: 30px;
    font-family: 宋体;margin-top: 20px">模拟试题--选择大题</div>
    <el-divider></el-divider>
    <el-table :data="tableData" style="width: 94%;margin-left:20px ;">
      <el-table-column label="题号" type="index" width="120px"></el-table-column>
      <el-table-column>
        <template slot-scope="scope">
          <div>
            <span>{{scope.row.tittle}}</span>
          </div>

          <div style="margin-top:18px">
            <el-checkbox-group v-model="scope.row.checkedSelect" @change="change">
              <el-checkbox label="A">{{scope.row.selectA}}</el-checkbox>
              <el-checkbox label="B">{{scope.row.selectB}}</el-checkbox>
              <el-checkbox label="C">{{scope.row.selectC}}</el-checkbox>
              <el-checkbox label="D">{{scope.row.selectD}}</el-checkbox>
            </el-checkbox-group>
          </div>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [],
      total: 0,
      addLoading: false
    };
  },
  props: ["topicType", "childMethods"],
  methods: {
    change(message) {
      this.childMethods.onChange1(this.tableData);
    },
    getList() {
      const self = this;
      self
        .$axios({
          methods: "get",
          url: "TopicInfo",
          params: {
            take: 9999,
            skip: 0,
            topicType: self.topicType
          }
        })
        .then(e => {
          self.tableData = e.data.items;
          for (const dto of self.tableData) {
            dto.checkedSelect = [];
          }
          self.total = e.data.total;
        })
        .catch(e => {
          console.log("失败");
        });
    }
  },
  created() {
    debugger;
    this.getList();
  }
};
</script>

<style>
</style>