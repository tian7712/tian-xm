<template>
  <div class="outside-div">
    <el-row :gutter="20">
      <el-col :span="8">
        <el-card class="box-card-1">
          <div class="card-div">
            <!-- <svg-icon icon-class="user1"/> -->
            <div class="card-background background1">
              <svg class="icon" aria-hidden="true">
                <use xlink:href="#icon-user1" />
              </svg>
            </div>
          </div>
          <div class="card-1-div">
            <span>0</span>
            <p>今日新增会员</p>
          </div>
        </el-card>
      </el-col>
      <el-col :span="8">
        <el-card class="box-card-1">
          <div class="card-div">
            <div class="card-background background2">
              <svg class="icon" aria-hidden="true">
                <use xlink:href="#icon-dingdan" />
              </svg>
            </div>
          </div>
          <div class="card-1-div">
            <span>0</span>
            <p>今日付款订单数</p>
          </div>
        </el-card>
      </el-col>
      <el-col :span="8">
        <el-card class="box-card-1">
          <div class="card-div">
            <div class="card-background background3">
              <svg class="icon" aria-hidden="true">
                <use xlink:href="#icon-money1" />
              </svg>
            </div>
          </div>
          <div class="card-1-div">
            <span>¥ 0</span>
            <p>今日付款金额</p>
          </div>
        </el-card>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="12">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>待处理事务</span>
          </div>

          <div class="affair">
            <el-row :gutter="20">
              <el-col :span="8">
                <ul class="affairui">
                  <li>
                    商品库存报警
                    <span>11</span>
                  </li>
                  <li>
                    待处理退款
                    <span>10</span>
                  </li>
                  <li>
                    仓库中商品
                    <span>1</span>
                  </li>
                </ul>
              </el-col>
              <el-col :span="8">
                <ul class="affairui">
                  <li>
                    待付款订单
                    <span>12</span>
                  </li>
                  <li>
                    待评价订单
                    <span>18</span>
                  </li>
                  <li>
                    待处理退货
                    <span>13</span>
                  </li>
                </ul>
              </el-col>
              <el-col :span="8">
                <ul class="affairui">
                  <li>
                    商品库存报警
                    <span>12</span>
                  </li>
                  <li>
                    待处理退款
                    <span>12</span>
                  </li>
                  <li>
                    仓库中商品
                    <span>13</span>
                  </li>
                </ul>
              </el-col>
            </el-row>
          </div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>交易统计</span>
          </div>
          <div class="chart-container">
            <el-row :gutter="20">
              <el-col :span="24">
                <chart height="200%" width="100%" />
                <!-- <div id="myChart" :style="{width:'300px',height:'300px'}"></div> -->
              </el-col>
            </el-row>
          </div>
        </el-card>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="12">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>商城信息统计</span>
          </div>
          <div>
            <el-row :gutter="20" class="Modular2">
              <el-col :span="8">
                <span>2226</span>
                <p>会员总数</p>
              </el-col>
              <el-col :span="8">
                <span>26</span>
                <p>商品总数</p>
              </el-col>
              <el-col :span="8">
                <span>226</span>
                <p>团长总数</p>
              </el-col>
            </el-row>
            <el-row :gutter="20" class="Modular2"  style="margin:5% 0">
              <el-col :span="8">
                <span>26</span>
                <p>近七天订单数</p>
              </el-col>
              <el-col :span="8">
                <span>43.12</span>
                <p>近七天销售额（元）</p>
              </el-col>
              <el-col :span="8">
                <span>126.22</span>
                <p>近七天退款金额（元）</p>
              </el-col>
            </el-row>
          </div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card class="box-card box-rk">
          <div slot="header" class="clearfix">
            <span>快捷入口</span>
          </div>
          <div>
            <el-row type="flex" class="row-bg" justify="space-around">
              <el-col :span="4">
                <!-- <svg-icon icon-class="tiaoxingma"/> -->
                <div class="card-icon">
                  <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-tiaoxingma" />
                  </svg>
                  
                </div>
                <p><a href="">发布商品</a></p>
              </el-col>
              <el-col :span="4">
                <div class="card-icon">
                  <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-dingdancg" />
                  </svg>
                </div>
                <p><a href="">订单列表</a></p>
              </el-col>
              <el-col :span="4">
                <div class="card-icon">
                  <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-user2" />
                  </svg>
                </div>
                <p><a href="">会员管理</a></p>
              </el-col>
              <el-col :span="4">
                <div class="card-icon">
                  <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-tree1" />
                  </svg>
                </div>
                <p><a href="">团长管理</a></p>
              </el-col>
              <el-col :span="4">
                <div class="card-icon">
                  <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-car1" />
                  </svg>
                </div>
                <p><a href="">供应商管理</a></p>
              </el-col>
            </el-row>
            <el-row type="flex" class="row-bg icon-row" justify="space-around" style="margin:5% 0">
              <el-col :span="4">
                <!-- <svg-icon icon-class="tiaoxingma"/> -->
                <div class="card-icon">
                  <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-tixian" />
                  </svg>
                  
                </div>
                <p><a href="">提现申请</a></p>
              </el-col>
              <el-col :span="4">
                <div class="card-icon">
                  <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-tixian2" />
                  </svg>
                </div>
                <p><a href="">提现设置</a></p>
              </el-col>
              <el-col :span="4">
                <div class="card-icon">
                  <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-huandengpian" />
                  </svg>
                </div>
                <p><a href="">幻灯片设置</a></p>
                
              </el-col>
              <el-col :span="4">
                <div class="card-icon">
                  <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-shouhou" />
                  </svg>
                </div>
                <p><a href="">售后设置</a></p>
              </el-col>
              <el-col :span="4">
                <div class="card-icon">
                  <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-shezhi" />
                  </svg>
                </div>
                <p><a href="">站点设置</a></p>
              </el-col>
              <!-- <el-col :span="4">提现申请</el-col>
              <el-col :span="4"></el-col>
              <el-col :span="4"></el-col>
              <el-col :span="4"></el-col>
              <el-col :span="4"></el-col> -->
            </el-row>
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>
<script>
import Chart from "@/components/Charts/LineMarker";

// let echarts =require("echarts/lib/echarts");
// require("echarts/lib/chart/bar");
// require("echarts/lib/components/tooltip");
// require("echarts/lib/components/title");

export default {
  name: "KeyboardChart",
  components: { Chart },
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
          caozuo: <el-button type="success">编辑</el-button>
        }
      ],
      value: ""
    };
  }
};
</script>

<style lang="scss" scoped>
.outside-div {
  margin: 1%;
  margin-top: 3%;
}
.el-row {
  margin-top: 1%;
}
.box-card {
  height: 300px;
  margin: 1%;
}
.box-card-1 {
  .el-avatar {
    margin: 1%;
  }
  .card-div {
    margin-left: 5%;
    padding: 10% 5%;

    float: left;
    // width: 70px;
    // height: 70px;
    // background-color: red;
    // border-radius:50%;
  }
  .card-1-div {
    // margin-left: 5%;
    padding: 7% 5%;
    span {
      font-size: 1.5em;
    }
   
  }
}

.affairui {
  li {
    list-style: none;
    border-bottom: 1px solid gainsboro;
    padding: 15% 0;
    color: gray;
    font-size: 0.85em;
    span {
      color: gray;
      font-weight: bold;
      float: right;
    }
  }
  li:nth-last-child(1) {
    border: none;
  }
}
.Modular2 {
  text-align: center;
  font-size: 0.7em;
  color: gray;
  p {
    padding: 5%;
  }
  span {
    color: black;

    font-size: 1.5em;
  }
}
// 表格
.chart-container {
  //  position: relative;
  width: 100%;
  // height:220px;
}
.icon {
  width: 2em;
  height: 2em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
.card-background {
  border-radius: 50%;
  width: 60px;
  height: 60px;
  display: flex;
  justify-content: center; /* 水平居中 */
  align-items: center;
}
.background1 {
  background-color: red;
}
.background2 {
  background-color: green;
}
.background3 {
  background-color: orchid;
}
.box-rk{
   p{
      margin: 0 auto;
      font-size: 0.7em;
      color: gray;
      // text-align: center;
      display: flex;
  justify-content: center; /* 水平居中 */
    }
     p:hover{
      color: skyblue;
    }
}
.card-icon {
  // width: 60px;
  height: 60px;
  display: flex;
  justify-content: center; /* 水平居中 */
  align-items: center;
}
// .icon-row{
//   el-col:hover{
// height: 60px;
//   color: skyblue;
//   background-color: skyblue ;
// }
//   }

  // width: 60px;
  
</style>
