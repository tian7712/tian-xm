<template>
  <div>
      <div class="position">当前位置：添加商品
          
          
      </div>
    <el-card class="box-card">
     <form action="Handler1.ashx" method="post" >
<p>商品名称<el-input v-model="input" placeholder="请输入内容" style="width: 300px" ></el-input></p>
<p>商品排序<el-input v-model="input" placeholder="请输入内容" style="width: 300px" ></el-input></p>
<p>商品类别<el-radio v-model="radio" :label="3">积分</el-radio></p>
<p>商品分类<el-select v-model="value" placeholder="请选择">
    
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value">
    </el-option>
  </el-select></p>
   <p>商品简介<el-input
  type="textarea"
  :rows="2"
  placeholder="请输入内容"
  v-model="textarea">
</el-input></p>
    <p>商品标题<el-input v-model="input" placeholder="请输入内容" style="width: 300px" ></el-input></p>
    <p>首页商品<el-input v-model="input" placeholder="请输入内容" style="width: 300px" ></el-input></p>
    <p>首页商品<el-input v-model="input" placeholder="请输入内容" style="width: 300px" ></el-input></p>
<!-- <p><input type="submit" value="提交" @click="open"/></p> -->
</form>
    </el-card>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tableData: [
        {
          id: "20",
          name: "1",
          price: "积分：30",
          salesvolume: 2,
          stock: 1,
          Uppershelf: "是否上架",
          caozuo: ""
        }
      ],
      radio: 3,
      value: "",
      switchvalue: "",
      options: [{
          value: '选项1',
          label: '全部商品'
        }, {
          value: '选项2',
          label: '水果蔬菜'
        }, {
          value: '选项3',
          label: '肉蛋禽类'
        }]
        // value: ''
    };
  },
   methods: {
      open() {
        this.$confirm('添加, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '添加成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消添加'
          });          
        });
      }
    }
};
</script>

<style lang="scss" scoped>
.position{
  margin: 3% 0 1% 1%;

}
.box-card {
  margin: 1%;
  margin-top: 1%;
  padding: 2%;
  .el-col {
    margin: 0 1%;
    // input{
    //     width: 100px;
    // }
  }
}
</style>
