<template>
  <div class="app-container">
    <el-card class="box-card">
      <div class="filter-container">
        <el-input
          v-model="listQuery.title"
          placeholder="输入商品编码或名称"
          style="width: 190px;"
          class="filter-item"
          @keyup.enter.native="handleFilter"
        />
        <el-select
          v-model="listQuery.importance"
          placeholder="不按时间"
          clearable
          style="width: 120px"
          class="filter-item"
        >
          <el-option v-for="item in importanceOptions" :key="item" :label="item" :value="item" />
        </el-select>
        <el-date-picker
          v-model="datevalue2"
          class="filter-item"
          type="daterange"
          align="right"
          unlink-panels
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          :picker-options="pickerOptions"
        ></el-date-picker>
        <el-select
          v-model="listQuery.importance"
          placeholder="商品分类"
          clearable
          style="width: 120px"
          class="filter-item"
        >
          <el-option v-for="item in importanceOptions" :key="item" :label="item" :value="item" />
        </el-select>
        <el-select
          v-model="listQuery.importance"
          placeholder="全部商品"
          clearable
          style="width: 120px"
          class="filter-item"
        >
          <el-option v-for="item in importanceOptions" :key="item" :label="item" :value="item" />
        </el-select>
        <el-button
          v-waves
          class="filter-item"
          type="primary"
          icon="el-icon-search"
          @click="handleFilter"
        >搜索</el-button>
        <el-button
          class="filter-item"
          style="margin-left: 10px;"
          type="primary"
          icon="el-icon-edit"
          @click="handleCreate"
        >添加</el-button>
      </div>

      <el-table
        :key="tableKey"
        v-loading="listLoading"
        :data="list"
        fit
        highlight-current-row
        style="width: 100%;"
        @sort-change="sortChange"
      >
        <el-table-column
          label="ID"
          prop="id"
          sortable="custom"
          align="center"
          width="80"
          :class-name="getSortClass('id')"
        >
          <template slot-scope="{row}">
            <span>{{ row.id }}</span>
          </template>
        </el-table-column>
        <el-table-column label="排序" width="100px" align="center">
          <template slot-scope="{row}">
            <span>{{ row.timestamp | parseTime('{y}-{m}-{d} {h}:{i}') }}</span>
          </template>
        </el-table-column>
        <el-table-column label="商品名称" min-width="150px">
          <template slot-scope="{row}">
            <span class="link-type" @click="handleUpdate(row)">{{ row.title }}</span>
            <el-tag>{{ row.type | typeFilter }}</el-tag>
          </template>
        </el-table-column>
        <el-table-column label="活动时间" width="150px" align="center">
          <template slot-scope="{row}">
            <span>{{ row.timestamp | parseTime('{y}-{m}-{d} {h}:{i}') }}</span>
          </template>
        </el-table-column>
        <el-table-column label="商品类别" width="110px" align="center">
          <template slot-scope="{row}">
            <span>{{ row.author }}</span>
          </template>
        </el-table-column>
        <el-table-column v-if="showReviewer" label="价格" width="110px" align="center">
          <template slot-scope="{row}">
            <span style="color:red;">{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="销量" width="80px">
          <template slot-scope="{row}">
            <span
              v-if="row.pageviews"
              class="link-type"
              @click="handleFetchPv(row.pageviews)"
            >{{ row.pageviews }}</span>
            <span v-else>0</span>
          </template>
        </el-table-column>
        <el-table-column label="库存" align="center" width="95">
          <template slot-scope="{row}">
            <span
              v-if="row.pageviews"
              class="link-type"
              @click="handleFetchPv(row.pageviews)"
            >{{ row.pageviews }}</span>
            <span v-else>0</span>
          </template>
        </el-table-column>
        <el-table-column label="是否上架" class-name="status-col" width="100">

         
         
       <el-switch v-model="temp" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
      
          
        </el-table-column>
        <el-table-column label="今日推荐" width="100px" align="center">
          <!-- <template slot-scope="{row}">
          <span>{{ row.timestamp | parseTime('{y}-{m}-{d} {h}:{i}') }}</span>
          </template>-->
          <el-switch v-model="item1" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
        </el-table-column>
        <el-table-column label="置顶" width="100px" align="center">
          <el-switch v-model="temp" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
        </el-table-column>
        <el-table-column
          label="操作"
          align="center"
          width="150"
          class-name="small-padding fixed-width"
        >
          <template slot-scope="{row,$index}">
            <el-button type="primary" size="mini" @click="handleUpdate(row)">修改</el-button>
            <!-- <el-button v-if="row.status!='published'" size="mini" type="success" @click="handleModifyStatus(row,'published')">
            Publish
          </el-button>
          <el-button v-if="row.status!='draft'" size="mini" @click="handleModifyStatus(row,'draft')">
            Draft
            </el-button>-->
            <el-button v-if="row.status!='deleted'" size="mini" type="danger" @click="handleDelete(row,$index)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>

      <pagination
        v-show="total>0"
        :total="total"
        :page.sync="listQuery.page"
        :limit.sync="listQuery.limit"
        @pagination="getList"
      />
    </el-card>
    <el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible">
      <el-form
        ref="dataForm"
        :rules="rules"
        :model="temp"
        label-position="left"
        label-width="100px"
        style="width: 500px; margin-left:50px;"
      >
        <el-form-item label="商品名称 " prop="title">
          <el-input v-model="temp.title" />
        </el-form-item>
        <el-form-item label="商品排序" prop="title">
          <el-input v-model="temp.title" />
        </el-form-item>
        <el-form-item label="商品分类" prop="type">
          <el-select v-model="temp.type" class="filter-item" placeholder="选择分类">
            <el-option
              v-for="item in calendarTypeOptions"
              :key="item.key"
              :label="item.display_name"
              :value="item.key"
            />
          </el-select>
        </el-form-item>
        <el-form-item label="时间" prop="timestamp">
          <el-date-picker
            v-model="temp.timestamp"
            type="datetime"
            placeholder="Please pick a date"
          />
        </el-form-item>
        <el-form-item label="商品简介">
          <el-input
            v-model="temp.remark"
            :autosize="{ minRows: 2, maxRows: 4}"
            type="textarea"
            placeholder="长度请控制在100字以内"
          />
        </el-form-item>
        <el-form-item label="商品简短标题" prop="小票打印机使用，请控制字数，未填写默认截取商品标题(无须打印的可以不填写)">
          <el-input v-model="temp.title" />
        </el-form-item>
        <!-- </el-form-item> -->
        <el-form-item label="首页商品图片（1）" prop="">
          
          <div :id="id" :ref="id" :action="url" class="dropzone drop">
            <input type="file" name="file" />
            <el-image style="width: 70px; height: 70px" :src="url"></el-image>
          </div>

          <!-- <img src={{url}} alt=""> -->
         <span>此图为首页商品大图模式封面图，最佳尺寸:680*380</span>
        </el-form-item>
        <el-form-item label="首页商品图片（2）" prop="">
          <div :id="id" :ref="id" :action="url" class="dropzone drop">
            <input type="file" name="file" />
            <el-image style="width: 70px; height: 70px" :src="url"></el-image>
          </div>

          <span>此图为首页商品大图模式封面图，最佳尺寸:680*380</span>
        </el-form-item>
        <el-form-item label="分享标题 " prop="">
          <el-input v-model="temp.title" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogStatus==='create'?createData():updateData()">确认</el-button>
        <el-button @click="dialogFormVisible = false">取消</el-button>
      </div>
    </el-dialog>

    <el-dialog :visible.sync="dialogPvVisible" title="Reading statistics">
      <el-table :data="pvData" border fit highlight-current-row style="width: 100%">
        <el-table-column prop="key" label="Channel" />
        <el-table-column prop="pv" label="Pv" />
      </el-table>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogPvVisible = false">确认</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import {
  fetchList,
  fetchPv,
  createArticle,
  updateArticle
} from "@/api/article";
import waves from "@/directive/waves"; // waves directive
import { parseTime } from "@/utils";
import Pagination from "@/components/Pagination"; // secondary package based on el-pagination基于el分页的二次包

import Dropzone from "dropzone";
import "dropzone/dist/dropzone.css";
// import { getToken } from 'api/qiniu';

Dropzone.autoDiscover = false;
// 上传图片

const calendarTypeOptions = [
  { key: "CN", display_name: "水果蔬菜" },
  { key: "US", display_name: "蛋肉禽类" },
  { key: "JP", display_name: "海鲜冻品" },
  { key: "EU", display_name: "米面盐油" }
];

// arr to obj, such as { CN : "China", US : "USA" }
const calendarTypeKeyValue = calendarTypeOptions.reduce((acc, cur) => {
  acc[cur.key] = cur.display_name;
  return acc;
}, {});

export default {
  name: "ComplexTable",
  components: { Pagination },
  directives: { waves },
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: "success",
        draft: "info",
        deleted: "danger"
      };
      return statusMap[status];
    },
    typeFilter(type) {
      return calendarTypeKeyValue[type];
    }
  },
  data() {
    return {
        item1:"",
      tableKey: 0,
      list: null,
      total: 0,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 20,
        importance: undefined,
        title: undefined,
        type: undefined,
        sort: "+id"
      },
      importanceOptions: [1, 2, 3],
      calendarTypeOptions,
      sortOptions: [
        { label: "ID Ascending", key: "+id" },
        { label: "ID Descending", key: "-id" }
      ],
      statusOptions: ["published", "draft", "deleted"],
      showReviewer: false,
      temp: {
        id: undefined,
        importance: 1,
        remark: "",
        timestamp: new Date(),
        title: "",
        type: "",
        status: "published"
      },
      dialogFormVisible: false,
      dialogStatus: "",
      textMap: {
        update: "修改",
        create: "添加"
      },
      dialogPvVisible: false,
      pvData: [],
      rules: {
        type: [
          { required: true, message: "type is required", trigger: "change" }
        ],
        timestamp: [
          {
            type: "date",
            required: true,
            message: "timestamp is required",
            trigger: "change"
          }
        ],
        title: [
          { required: true, message: "title is required", trigger: "blur" }
        ]
      },
      downloadLoading: false,
      pickerOptions: {
        shortcuts: [
          {
            text: "最近一周",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit("pick", [start, end]);
            }
          },
          {
            text: "最近一个月",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
              picker.$emit("pick", [start, end]);
            }
          },
          {
            text: "最近三个月",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
              picker.$emit("pick", [start, end]);
            }
          }
        ]
      },
      datevalue2: ""
      //    fits: ['fill'],
      //     url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg'
    };
  },
  created() {
    this.getList();
  },
  methods: {
    getList() {
      this.listLoading = true;
      fetchList(this.listQuery).then(response => {
        this.list = response.data.items;
        this.total = response.data.total;

        // Just to simulate the time of the request
        setTimeout(() => {
          this.listLoading = false;
        }, 1.5 * 1000);
      });
    },
    handleFilter() {
      this.listQuery.page = 1;
      this.getList();
    },
    handleModifyStatus(row, status) {
      this.$message({
        message: "操作成功",
        type: "success"
      });
      row.status = status;
    },
    sortChange(data) {
      const { prop, order } = data;
      if (prop === "id") {
        this.sortByID(order);
      }
    },
    sortByID(order) {
      if (order === "ascending") {
        this.listQuery.sort = "+id";
      } else {
        this.listQuery.sort = "-id";
      }
      this.handleFilter();
    },
    resetTemp() {
      this.temp = {
        id: undefined,
        importance: 1,
        remark: "",
        timestamp: new Date(),
        title: "",
        status: "published",
        type: ""
      };
    },
    handleCreate() {
      this.resetTemp();
      this.dialogStatus = "create";
      this.dialogFormVisible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].clearValidate();
      });
    },
    createData() {
      this.$refs["dataForm"].validate(valid => {
        if (valid) {
          this.temp.id = parseInt(Math.random() * 100) + 1024; // mock a id
          this.temp.author = "vue-element-admin";
          createArticle(this.temp).then(() => {
            this.list.unshift(this.temp);
            this.dialogFormVisible = false;
            this.$notify({
              title: "添加商品",
              message: "添加成功",
              type: "success",
              duration: 2000
            });
          });
        }
      });
    },
    handleUpdate(row) {
      this.temp = Object.assign({}, row); // copy obj
      this.temp.timestamp = new Date(this.temp.timestamp);
      this.dialogStatus = "update";
      this.dialogFormVisible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].clearValidate();
      });
    },
    updateData() {
      this.$refs["dataForm"].validate(valid => {
        if (valid) {
          const tempData = Object.assign({}, this.temp);
          tempData.timestamp = +new Date(tempData.timestamp); // change Thu Nov 30 2017 16:41:05 GMT+0800 (CST) to 1512031311464
          updateArticle(tempData).then(() => {
            const index = this.list.findIndex(v => v.id === this.temp.id);
            this.list.splice(index, 1, this.temp);
            this.dialogFormVisible = false;
            this.$notify({
              title: "修改",
              message: "修改成功",
              type: "success",
              duration: 2000
            });
          });
        }
      });
    },
    handleDelete(row, index) {
      this.$notify({
        title: "删除",
        message: "删除成功",
        type: "success",
        duration: 2000
      });
      this.list.splice(index, 1);
    },
    handleFetchPv(pv) {
      fetchPv(pv).then(response => {
        this.pvData = response.data.pvData;
        this.dialogPvVisible = true;
      });
    },
    handleDownload() {
      this.downloadLoading = true;
      import("@/vendor/Export2Excel").then(excel => {
        const tHeader = ["timestamp", "title", "type", "importance", "status"];
        const filterVal = [
          "timestamp",
          "title",
          "type",
          "importance",
          "status"
        ];
        const data = this.formatJson(filterVal);
        excel.export_json_to_excel({
          header: tHeader,
          data,
          filename: "table-list"
        });
        this.downloadLoading = false;
      });
    },
    formatJson(filterVal) {
      return this.list.map(v =>
        filterVal.map(j => {
          if (j === "timestamp") {
            return parseTime(v[j]);
          } else {
            return v[j];
          }
        })
      );
    },
    getSortClass: function(key) {
      const sort = this.listQuery.sort;
      return sort === `+${key}` ? "ascending" : "descending";
    }
  }
};
</script>

<style scoped>
.drop{
    border:1px solid gainsboro;
}
</style>
