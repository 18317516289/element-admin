<template>
  <div>
    <div style="margin-left: 800px;font-size: 30px;
    font-family: 宋体;margin-top: 20px">模拟试题--判断大题</div>
    <el-divider></el-divider>
    <el-table :data="tableData" style="width: 94%;margin-left:20px ;">
      <el-table-column label="题号" type="index" width="120px"></el-table-column>
      <el-table-column>
        <template slot-scope="scope">
          <div>
            <span>{{scope.row.tittle}}</span>
          </div>

          <div style="margin-top:18px">
            <el-checkbox-group v-model="scope.row.checked" @change="change">
              <el-checkbox :label="true">{{scope.row.trueContext}}</el-checkbox>
              <el-checkbox :label="false">{{scope.row.falseContext}}</el-checkbox>
            
            </el-checkbox-group>
          </div>
        </template>
      </el-table-column>
    </el-table>
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
export default {
  data() {
    return {
      tableData: [],
      total:0,
      addLoading:false
    };
  },
  methods: {
    change(message) {
        debugger
    },

    OnSubmit() {
      self.addLoading = true
      
         self.addLoading = false},
    getList() {
      const self = this;
      self.$axios({
        methods: "get",
        url: "TopicInfo",
        params: {
          take: 9999,
          skip: 0,
          topicType:1
        }
      }).then(e=>{
        self.tableData = e.data.items
        for (const dto of self.tableData) {
            dto.checked=[]
        }
        self.total = e.data.total
      }).catch(e=>{
     console.log("失败")
      })
    }
  },
  created(){
this.getList()
     
  }
};
</script>

<style>
</style>