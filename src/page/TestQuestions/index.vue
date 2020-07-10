<template>
  <div>
    <div style="margin-left: 800px;font-size: 30px;
    font-family: 宋体;margin-top: 20px">模拟试题</div>
    <el-divider></el-divider>
    <el-table :data="tableData" style="width: 94%;margin-left:20px ;">
      <el-table-column label="题号" type="index" width="120px"></el-table-column>
      <el-table-column >
        <template slot-scope="scope">
          <div><span>{{scope.row.address}}</span></div>
        
           <div style="margin-top:18px">
          <el-checkbox-group v-model="scope.row.checked" @change="change" :name="scope.row.name">
            <el-checkbox label="复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A复选框 A"></el-checkbox>
            <el-checkbox :label="scope.row.address"></el-checkbox>
            <el-checkbox label="复选框 C"></el-checkbox>
            <el-checkbox label="禁用" disabled></el-checkbox>
            <el-checkbox label="选中且禁用" disabled></el-checkbox>
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
      tableData: [
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
          checked:[]
        },
        {
          date: "2016-05-04",
          name: "王小二",
          address: "上海市普陀区金沙江路 1517 弄",
          checked:[]
        },
        {
          date: "2016-05-01",
          name: "王小三",
          address: "上海市普陀区金沙江路 1519 弄",
          checked:[]
        },
        {
          date: "2016-05-03",
          name: "王小四",
          address: "上海市普陀区金沙江路 1516 弄",
          checked:[]
        }
      ],
        checkList: []
    };
  },
  methods:{
      change(e,s,d){
          debugger;
          console.log(e);
      }
  }
};
</script>

<style>
</style>