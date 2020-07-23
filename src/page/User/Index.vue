<template>
  <div>
    <el-form :inline="true" style="margin-left:20px">
      <br />
      <el-form-item label="性别" prop="Sexs" width="120px">
        <el-select v-model="query.sex" placeholder="请选择">
          <el-option v-for="item in Sexs" :key="item.value" :label="item.label" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="角色" prop="Roles">
        <el-select v-model="query.roleId" placeholder="请选择">
          <el-option
            v-for="item in Roles"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="用户名" width="120px">
        <el-input v-model="query.name" placeholder="请输入姓名"></el-input>
      </el-form-item>
      <el-form-item label="电话">
        <el-input v-model="query.phone" placeholder="请输入电话"></el-input>
      </el-form-item>
      <el-button type="primary" style="margin-left:5px" @click="getList()" :loading="getLoading">搜索</el-button>
    </el-form>
    <el-button type="button" style="margin-left:20px" @click="onAdd()">添加用户</el-button>
    <el-divider></el-divider>
    <el-table :data="tableData" style="width: 94%;margin-left:20px ">
      <el-table-column label="编号" type="index" width="150px"></el-table-column>
      <el-table-column label="用户名" prop="name"></el-table-column>
      <el-table-column label="性别" prop="sex">
        <template slot-scope="scope">{{scope.row.sex|sexFor}}</template>
      </el-table-column>
      <el-table-column label="角色" prop="roleId">
        <template slot-scope="scope">{{scope.row.roleId|rolefor}}</template>
      </el-table-column>
      <el-table-column label="电话" prop="phone"></el-table-column>
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
    </div>
  </div>
</template>

<script>
import add from "./components/add-user";
import edit from "./components/edit-user";

export default {
  components: { add, edit },
  data() {
    return {
      content: null,

      editorOption: {},
      tableData: [],
      total: 0,
      query: {
        name: "",
        sex: null,
        roleId: null,
        phone: ""
      },
      Sexs: [
        {
          value: 0,
          label: "男"
        },
        {
          value: 1,
          label: "女"
        },
        {
          value: null,
          label: "请选择"
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
        },
        {
          value: null,
          label: "请选择"
        }
      ],
      getLoading: false
    };
  },
  methods: {
    getList() {
      const self = this;
      self.getLoading = true;
      self
        .$axios({
          methods: "get",
          url: "User",
          params: {
            take: 5,
            skip: 0,
            ...self.query
          }
        })
        .then(e => {
          self.tableData = e.data.items;
          self.total = e.data.total;
          self.getLoading = false;
        })
        .catch(e => {
          self.getLoading = false;
          self.$message.error(e);
        });
    },
    onAdd() {
      const self = this;
      self.$refs["add-action"].show();
    },
    onDelete(id) {
      const self = this;
      self.$axios({
        methods: "delete",
        url: "/User",
        params: {
          id: id
        }
      });
    },
    onUpdate(row) {
      const self = this;
      self.$refs["edit-action"].show(row);
    }
  },
  filters: {
    sexFor(sex) {
      if (sex === 1) {
        return "女";
      } else {
        return "男";
      }
    },
    rolefor(roleId) {
      if (roleId === 0) {
        return "用户";
      } else {
        return "管理员";
      }
    }
  },
  created() {
    this.getList();
  }
};
</script>

<style>
</style>