<template>
  <div class="app-container">
    <div class="position">当前位置：物资管理
          
          
      </div>
    <el-card class="box-card">
      
      <el-row :gutter="20">
        <el-col :span="4">
          <el-input placeholder="请输入内容" clearable></el-input>
        </el-col>
        <el-col :span="4">
          <el-select v-model="value" placeholder="请选择">
            <el-option v-for="item in options" :key="item.value" :value="item.value"></el-option>
          </el-select>
        </el-col>
        <el-col :span="2">
          <el-button type="success" icon="el-icon-search">搜索</el-button>
        </el-col>

        <el-col :span="2" :offset="12">
          <el-button type="primary" @click="handleAddRole">添加</el-button>
        </el-col>
      </el-row>

      <el-table :data="rolesList" style="width: 100%;margin-top:30px;">
        <el-table-column align="center" label="ID" width="50">
          <template slot-scope="scope">{{ scope.row.key }}</template>
        </el-table-column>
        <el-table-column align="center" label="图片" width="200">
         <el-image :src="url" style="width: 60px; height:60px;">
      <div slot="error" class="image-slot">
        <i class="el-icon-picture-outline"></i>
      </div>
    </el-image>
        </el-table-column>
        <el-table-column align="header-center" label="名称">
          <template slot-scope="scope">{{ scope.row.description }}</template>
        </el-table-column>

        <el-table-column align="header-center" label="规格" width="120">
          <template>1</template>
        </el-table-column>
        <el-table-column align="header-center" label="是否上架" width="100">
          <el-switch v-model="swvalue" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
          <!-- v-model="？"的值要循环吧？不然不能用 -->
          <!-- <template slot-scope="scope">{{ scope.row.description }}</template> -->
        </el-table-column>
        <el-table-column align="center" label="操作">
          <template slot-scope="scope">
            <el-button type="primary" size="small" @click="handleEdit(scope)">修改</el-button>
            <el-button type="danger" size="small" @click="handleDelete(scope)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-row :gutter="20">
  <el-col :span="4" :offset="21"><el-button style="margin:4%">共<span>1</span>条记录</el-button></el-col>
</el-row>
    </el-card>
    <el-dialog :visible.sync="dialogVisible" :title="dialogType==='edit'?'修改':'添加'">
      <el-form :model="role" label-width="80px" label-position="left">
        <el-form-item label="物资名称">
          <el-input v-model="role.name" placeholder="物资名称" />
        </el-form-item>
        <el-form-item label="物资图片">
          <el-input v-model="role.img" placeholder="物资图片">
            <el-button slot="append" autofocus>选择图片</el-button>
          </el-input>
        </el-form-item>
        <el-form-item label="物资规格">
          <el-input v-model="role.description" placeholder="物资规格" />
          <p>多规格之间用 / 分隔</p>
        </el-form-item>
        <el-form-item label="物资状态">
          <el-radio v-model="radio" label="1">上架</el-radio>
          <el-radio v-model="radio" label="2">下架</el-radio>
        </el-form-item>
      </el-form>
      <div style="text-align:right;">
        <el-button type="primary" @click="confirmRole">确认</el-button>
        <el-button type="danger" @click="dialogVisible=false">取消</el-button>
      </div>
    </el-dialog>
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
      role: Object.assign({}, defaultRole),
      routes: [],
      rolesList: [],
      dialogVisible: false,
      dialogType: "new",
      checkStrictly: false,
      defaultProps: {
        children: "children",
        label: "title"
      },
      options: [
        {
          value: "上架",
          label: "上架"
        },
        {
          value: "下架",
          label: "下架"
        }
      ],
      value: "",
      radio: "1",
      swvalue: true
    };
  },
  computed: {
    routesData() {
      return this.routes;
    }
  },
  created() {
    // Mock: get all routes and roles list from server
    this.getRoutes();
    this.getRoles();
  },
  methods: {
    async getRoutes() {
      const res = await getRoutes();
      this.serviceRoutes = res.data;
      this.routes = this.generateRoutes(res.data);
    },
    async getRoles() {
      const res = await getRoles();
      this.rolesList = res.data;
    },

    // Reshape the routes structure so that it looks the same as the sidebar
    generateRoutes(routes, basePath = "/") {
      const res = [];

      for (let route of routes) {
        // skip some route
        if (route.hidden) {
          continue;
        }

        const onlyOneShowingChild = this.onlyOneShowingChild(
          route.children,
          route
        );

        if (route.children && onlyOneShowingChild && !route.alwaysShow) {
          route = onlyOneShowingChild;
        }

        const data = {
          path: path.resolve(basePath, route.path),
          title: route.meta && route.meta.title
        };

        // recursive child routes
        if (route.children) {
          data.children = this.generateRoutes(route.children, data.path);
        }
        res.push(data);
      }
      return res;
    },
    generateArr(routes) {
      let data = [];
      routes.forEach(route => {
        data.push(route);
        if (route.children) {
          const temp = this.generateArr(route.children);
          if (temp.length > 0) {
            data = [...data, ...temp];
          }
        }
      });
      return data;
    },
    handleAddRole() {
      this.role = Object.assign({}, defaultRole);
      // console.log(this.role)
      if (this.$refs.tree) {
        this.$refs.tree.setCheckedNodes([]);
      }
      this.dialogType = "new";
      this.dialogVisible = true;
    },
    handleEdit(scope) {
      this.dialogType = "edit";
      this.dialogVisible = true;
      this.checkStrictly = true;
      this.role = deepClone(scope.row);
      this.$nextTick(() => {
        const routes = this.generateRoutes(this.role.routes);
        this.$refs.tree.setCheckedNodes(this.generateArr(routes));
        // set checked state of a node not affects its father and child nodes
        this.checkStrictly = false;
      });
    },
    handleDelete({ $index, row }) {
      this.$confirm("确定要删除吗?", "警告", {
        confirmButtonText: "确认",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(async () => {
          await deleteRole(row.key);
          this.rolesList.splice($index, 1);
          this.$message({
            type: "success",
            message: "Delete succed!"
          });
        })
        .catch(err => {
          console.error(err);
        });
    },
    generateTree(routes, basePath = "/", checkedKeys) {
      const res = [];

      for (const route of routes) {
        const routePath = path.resolve(basePath, route.path);

        // recursive child routes
        if (route.children) {
          route.children = this.generateTree(
            route.children,
            routePath,
            checkedKeys
          );
        }

        if (
          checkedKeys.includes(routePath) ||
          (route.children && route.children.length >= 1)
        ) {
          res.push(route);
        }
      }
      return res;
    },
    async confirmRole() {
      const isEdit = this.dialogType === "edit";

      const checkedKeys = this.$refs.tree.getCheckedKeys();
      this.role.routes = this.generateTree(
        deepClone(this.serviceRoutes),
        "/",
        checkedKeys
      );

      if (isEdit) {
        await updateRole(this.role.key, this.role);
        for (let index = 0; index < this.rolesList.length; index++) {
          if (this.rolesList[index].key === this.role.key) {
            this.rolesList.splice(index, 1, Object.assign({}, this.role));
            break;
          }
        }
      } else {
        const { data } = await addRole(this.role);
        this.role.key = data.key;
        this.rolesList.push(this.role);
      }

      const { description, key, name } = this.role;
      this.dialogVisible = false;
      this.$notify({
        title: "Success",
        dangerouslyUseHTMLString: true,
        message: `
            <div>Role Key: ${key}</div>
            <div>Role Name: ${name}</div>
            <div>Description: ${description}</div>
          `,
        type: "success"
      });
    },
    // reference: src/view/layout/components/Sidebar/SidebarItem.vue
    onlyOneShowingChild(children = [], parent) {
      let onlyOneChild = null;
      const showingChildren = children.filter(item => !item.hidden);

      // When there is only one child route, the child route is displayed by default
      if (showingChildren.length === 1) {
        onlyOneChild = showingChildren[0];
        onlyOneChild.path = path.resolve(parent.path, onlyOneChild.path);
        return onlyOneChild;
      }

      // Show parent if there are no child route to display
      if (showingChildren.length === 0) {
        onlyOneChild = { ...parent, path: "", noShowingChildren: true };
        return onlyOneChild;
      }

      return false;
    }
  }
};
</script>

<style lang="scss" scoped>
.position{
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
