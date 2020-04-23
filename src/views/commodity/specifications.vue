
<template>
  <div class="app-container">
    <div class="position">当前位置：商品分类</div>
    <el-card class="box-card">
      <el-button style="float: right; padding: 3px 0" type="text" @click="add">添加分类</el-button>

      <!-- <div v-for="item in 5" :key="item" class="text-item">
        {{'[ID: ' + item +']'}} 全部商品
        <div class="item-button">
          <el-switch v-model="list" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
          <el-button size="mini" @click="dialogFormVisible = true">修改</el-button>
          <el-button size="mini" @click="dialogFormVisible = true">删除</el-button>
        </div>
      </div> -->    
      <el-table
    ref="multipleTable"
    :data="tableData"
    tooltip-effect="dark"
    style="width: 100%"
    @selection-change="handleSelectionChange">
    <el-table-column
      type="selection"
      width="55">
    </el-table-column>
    <!-- <el-table-column
      label="日期"
      width="120">
      <template slot-scope="scope">{{ scope.row.date }}</template>
    </el-table-column> -->
    <el-table-column
      prop="name"
      label="规格名称"
      width="120">
    </el-table-column>
    
    <el-table-column
    prop="zhi"
      label="具体值"
      >
    </el-table-column>
    <el-table-column
      label="操作"
      width="200">
      <el-button size="mini" @click="edit">修改</el-button>
       <el-button size="mini" @click="del">删除</el-button>
      <!-- show-overflow-tooltip -->
    </el-table-column>
  </el-table>

      <el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible">
        <el-form :model="form">
          <el-form-item label="分类名称" :label-width="formLabelWidth">
            <el-input v-model="form.name" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="分类排序" :label-width="formLabelWidth">
            <el-input v-model="form.id" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="是否显示" :label-width="formLabelWidth">
            <el-radio v-model="form.radio" label="true">是</el-radio>
            <el-radio v-model="form.radio" label="false">否</el-radio>
          </el-form-item>
          <el-form-item label="分类图" :label-width="formLabelWidth">
            <div :id="id" :ref="id" :action="url" class="dropzone drop-div">
              <input type="file" name="file" />
              <el-image style="width: 70px; height: 70px" :src="url"></el-image>
            </div>
          </el-form-item>
          <el-form-item label="LOGO" :label-width="formLabelWidth">
            <div :id="id" :ref="id" :action="url" class="dropzone drop-div">
              <input type="file" name="file" />
              <el-image style="width: 70px; height: 70px" :src="url"></el-image>
            </div>
          </el-form-item>
          <!-- <el-form-item label="活动区域" :label-width="formLabelWidth">
        <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
        </el-select>
          </el-form-item>-->
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="primary" @click="dialogFormVisible = true">确 定</el-button>
          <el-button @click="dialogFormVisible = false">取 消</el-button>
        </div>
      </el-dialog>
    </el-card>
  </div>
</template>
<style lang="scss" scoped>
.position {
  margin: 3% 0 1% 1%;
}
.text-item {
  width: 80%;
  height: 50px;
  border: 1px solid gainsboro;
  margin: 1%;
  border-radius: 10px;
  padding: 1%;
  font-size: 0.8em;
}
.item-button {
  float: right;
}
.drop-div {
  width: 60%;
  border: 1px solid gainsboro;
  display: flex;
  // justify-content: center;
  align-items: center;
  padding: 1%;
}
</style>  
<script>
export default {
  data() {
    return {
      list: false,
      dialogTableVisible: false,
      dialogFormVisible: false,
      textMap: {
        update: "修改",
        create: "添加"
      },
      form: {
        name: "全部商品",
        radio: true,
        id: 2,
        img1: "",
        img2: ""
       
      },
      formLabelWidth: "120px",
       tableData: [{
          
          name: '重量',
          zhi: '400g,500g,1kg,1.5kg,2kg,2.5kg,3kg'
        }],
        multipleSelection: [],
        
    };
  },
  methods: {
      add(){
           this.dialogFormVisible = true;
         
    //   this.dialogStatus = create;
      this.dialogStatus = "create";
    //    this.resetTemp();
        
    //   this.$nextTick(() => {
    //     this.$refs["dataForm"].clearValidate();
    //   });

      },
      edit(){
          this.dialogFormVisible = true;
          this.dialogStatus = "update";

      },
      del() {
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      }
    }
};
</script>  