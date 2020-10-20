<template>
  <div>
    <el-form :inline="true" style="margin-left:20px">
      <br />
      <el-table-column label="请选择" prop="options">
        <el-select v-model="topicType" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
        <el-button
          type="primary"
          style="margin-left:5px"
          @click="getList()"
          :loading="getLoading"
        >搜索</el-button>
      </el-table-column>
    </el-form>
    <el-divider></el-divider>
    <el-table-column>
      <el-button type="button"  style="margin-left:20px" @click="onAdd()">添加试题</el-button>
    </el-table-column>
    <el-divider></el-divider>
    <el-table :data="tableData" style="width: 100%">
      <el-table-column type="expand">
        <template slot-scope="props"> git checkout -b wxssys 
          <el-form label-position="left" inline class="demo-table-expand">
            <el-form-item label="题目">
              <span>{{ props.row.tittle }}</span>
            </el-form-item>
            <el-form-item>
              <span></span>
            </el-form-item>
            <span v-if="props.row.topicType==0">
              <el-form-item label="答案A">
                <span>{{ props.row.selectA }}</span>
              </el-form-item>
              <el-form-item label="答案B">
                <span>{{ props.row.selectB }}</span>
              </el-form-item>
              <el-form-item label="答案C">
                <span>{{ props.row.selectC }}</span>
              </el-form-item>
              <el-form-item label="答案D">
                <span>{{ props.row.selectD }}</span>
              </el-form-item>
            </span>
            <span v-else>
              <el-form-item label="正确：">
                <span>{{ props.row.trueContext }}</span>
              </el-form-item>
              <el-form-item label="错误：">
                <span>{{ props.row.falseContext }}</span>
              </el-form-item>
            </span>
          </el-form>
        </template>
      </el-table-column>
      <el-table-column label="题号" type="index" width="150px"></el-table-column>
      <el-table-column label="题目" prop="tittle"></el-table-column>
      <el-table-column label="正确答案" width="150px">
        <template slot-scope="scope">
          <span v-if="scope.row.topicType==0">{{scope.row.winSelectResult}}</span>
          <span v-else>{{scope.row.winIsTrueResult |result}}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="150px">
        <template slot-scope="scope">
          <el-button type="button" size="mini" @click="onDelete(scope.row.id)">删除</el-button>
          <el-button type="button" size="mini" @click="onUpdate(scope.row)">修改</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div>
      <edit ref="edit-action" @submit="getList()"></edit>
      <add ref="add-action" @submit="getList()"></add>
      <el-dialog :visible.sync="visible" width="320px">
        <span>请选择题目类型</span>
        <div style="margin-top:35px">
          <el-radio-group v-model="type">
            <el-radio :label="0">选择题</el-radio>
            <el-radio :label="1">判断题</el-radio>
          </el-radio-group>
          <el-button type="button" size="small" style="margin-left:45px" @click="run()">确认</el-button>
        </div>
      </el-dialog>
    </div>
  </div>
</template>
<script>
import edit from "./components/update-topic";
import add from "./components/add-topic";
export default {
  components: { edit, add },
  data() {
    return {
      tableData: [],
      total: 0,
      type: 0,
      visible: false,
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
      topicType: null,
      getLoading: false
    };
  },
  methods: {
    getList() {
      const self = this;
      self.getLoading = true;
      this.$axios({
        method: "get",
        url: "/TopicInfo",
        params: {
          topicType: self.topicType,
          take: 5,
          skip: 0
        }
      })
        .then(res => {
          self.tableData = res.data.items;
          self.total = res.data.total;
          self.getLoading = false;
        })
        .catch(error => {
          this.$message.error(error);
          self.getLoading = false;
        });
    },

    onAdd() {
      this.visible = true;
    },
    run() {
      const self = this;
      if (self.type >= 0) {
        self.visible = false;
        debugger
        self.$refs["add-action"].show(self.type);
      } else {
        return;
      }
    },
    onDelete(num) {
      const self = this;
      self
        .$axios({
          method: "delete",
          url: "TopicInfo",
          params: {
            id: num
          }
        })
        .then(e => {
          self.getList();
        })
        .catch(e => {
          this.$message.error(e);
          self.getList();
        });
    },
    onUpdate(dto) {
      const self = this;
      self.$refs["edit-action"].show(dto);
    }
  },
  filters: {
    result(str) {
      if (str == true) return "正确";
      else {
        return "错误";
      }
    }
  },
  created() {
    const self = this;
    self.getList();
  }
};
</script>
<style>
.demo-table-expand {
  font-size: 0;
}
.demo-table-expand label {
  width: 90px;
  color: #99a9bf;
}
.demo-table-expand .el-form-item {
  margin-right: 0;
  margin-bottom: 0;
  width: 50%;
}
</style>