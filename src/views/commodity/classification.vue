
<template>
  <div class="app-container">
    <div class="position">当前位置：商品分类</div>
    <el-card class="box-card">
      <el-button style="float: right; padding: 3px 0" type="text" @click="dialogFormVisible = true">添加分类</el-button>

      <div v-for="item in 5" :key="item" class="text-item">
        {{'[ID: ' + item +']'}} 全部商品
        <div class="item-button">
          <el-switch v-model="list" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
          <el-button size="mini" @click="dialogFormVisible = true">修改</el-button>
          <el-button size="mini" @click="dialogFormVisible = true">删除</el-button>
        </div>
      </div>

      <el-dialog title="编辑分类" :visible.sync="dialogFormVisible">
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
          <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
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
      form: {
        name: "全部商品",
        radio: true,
        id: 2,
        img1: "",
        img2: ""
      },
      formLabelWidth: "120px"
    };
  }
};
</script>  