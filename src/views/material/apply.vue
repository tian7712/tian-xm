<template>
  <div class="app-container">
    <div class="position">当前位置：物资申请</div>
    <el-card class="box-card">
      <el-table :data="tableData" style="width: 100%">
        <el-table-column prop="ID" label="ID" width="50"></el-table-column>
        <el-table-column prop="img" label="图片" width="130"></el-table-column>
        <el-table-column prop="name" label="名称" width="120"></el-table-column>
        <el-table-column prop="num" label="数量" width="100"></el-table-column>
        <el-table-column prop="guige" label="规格" width="100"></el-table-column>
        <el-table-column prop="shequname" label="社区名" width="100"></el-table-column>
        <el-table-column prop="tuanname" label="团长名" width="100"></el-table-column>
        <el-table-column prop="address" label="地址" width="250"></el-table-column>
        <el-table-column prop="phone" label="手机" width="150"></el-table-column>
        <el-table-column prop="state" label="状态"></el-table-column>
      </el-table>
      <el-row :gutter="20">
        <el-col :span="4" :offset="21">
          <el-button style="margin:4%">
            共
            <span>1</span>条记录
          </el-button>
        </el-col>
      </el-row>
    </el-card>
  </div>
</template>

<script>
import path from "path";
import { deepClone } from "@/utils";
import {
  getRoutes,
  getRoles,
  addRole,
  deleteRole,
  updateRole
} from "@/api/role";

const defaultRole = {
  key: "",
  name: "",
  description: "",
  routes: []
};

export default {
  data() {
    return {
      tableData: [
        {
          ID: "20",
          name: "百岁山矿泉水",
          address: "上海市普陀区金沙江路 1518",
          num: 1,
          guige: "规格B",
          shequname: "聚客隆",
          tuanname: "测试",
          phone: 13016754310,
          state: "申请中"
        }
      ]
    };
  }
};
</script>

<style lang="scss" scoped>
.position {
  margin: 3% 0 1% 1%;
}
.app-container {
  .roles-table {
    margin-top: 30px;
  }
  .permission-tree {
    margin-bottom: 30px;
  }
}
</style>
