<template>
  <div class="pos">
    <div>
      <el-row >
        <el-col :span='7' class="pos-order" id="order-list">
          <el-tabs>

            <el-tab-pane label="点餐">
              <el-table :data="tableData" border style="width: 100%">

                <el-table-column prop="goodsName" label="商品"></el-table-column>

                <el-table-column prop="count" label="数量"></el-table-column>

                <el-table-column prop="price" label="金额"></el-table-column>

                <el-table-column label="操作" width="100px" fixed="right">
                  <template scope="scope">
                    <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>

              </el-table>

              <div class="totalDiv">
                <small>数量：</small>{{totalCount}}　　　　　　<small>金额：</small>{{totalMoney}}<small>元</small>
              </div>
              <div class="div-btn">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click="delAllGoods()">删除</el-button>
                <el-button type="success" @click="checkout()">结账</el-button>
              </div>
            </el-tab-pane>

            <el-tab-pane label="挂单">
              挂单
            </el-tab-pane>

            <el-tab-pane label="外卖">
              外卖
            </el-tab-pane>

          </el-tabs>



        </el-col>
        <!--商品展示-->
        <el-col :span="17">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-list">
              <ul>
                <li v-for="goods in oftengoods" @click="addOrderList(goods)">
                  <span>{{goods.goodsName}}</span>
                  <span class="often-price">￥{{goods.price}}元</span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <div>
                  <ul class='cookList'>
                    <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"/></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>

              <el-tab-pane label="小食">
                <div>
                  <ul class='cookList'>
                    <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"/></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>

              <el-tab-pane label="饮料">
                <div>
                  <ul class='cookList'>
                    <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"/></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>

              <el-tab-pane label="套餐">
                <div>
                  <ul class='cookList'>
                    <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"/></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'pos',
  data () {
    return {
      tableData: [],
      oftengoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalMoney: 0,
      totalCount: 0
    }
  },
  created: function () {
    axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(response => {
        console.log(response)
        this.oftengoods = response.data
      })
      .catch(error => {
        console.log(error)
        alert('网络错误，不能访问')
      })
    axios.get('http://jspang.com/DemoApi/typeGoods.php')
      .then(response => {
        console.log(response)
        this.type0Goods = response.data[0]
        this.type1Goods = response.data[1]
        this.type2Goods = response.data[2]
        this.type3Goods = response.data[3]
      })
      .catch(error => {
        console.log(error)
        alert('网络错误，不能访问')
      })
  },
  mounted: function () {
    var orderHeght = document.body.clientHeight
    document.getElementById('order-list').style.height = orderHeght + 'px'
  },
  methods: {
    addOrderList (goods) {
      this.totalMoney = 0
      this.totalCount = 0
      let isHave = false
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId === goods.goodsId) {
          isHave = true
        }
      }
      if (isHave) {
        let arr = this.tableData.filter(a => a.goodsId === goods.goodsId)
        arr[0].count ++
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        }
        this.tableData.push(newGoods)
      }
      this.getAllMoney()
    },
    delSingleGoods (goods) {
      console.log(goods)
      this.tableData = this.tableData.filter(o => o.goodsId !== goods.goodsId)
      this.getAllMoney()
    },
    delAllGoods () {
      this.tableData = []
      this.totalCount = 0
      this.totalMoney = 0
    },
    getAllMoney () {
      this.totalCount = 0
      this.totalMoney = 0
      if (this.tableData) {
        this.tableData.forEach((element) => {
          this.totalCount += element.count
          this.totalMoney = this.totalMoney + (element.count * element.price)
        })
      }
    },
    checkout () {
      if (this.totalCount !== 0) {
        this.tableData = []
        this.totalCount = 0
        this.totalMoney = 0
        this.$message({
          message: '结账成功！',
          type: 'success'
        })
      } else {
        this.$message.error('请先选择商品！')
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos-order{
  background: #f9fafc;
  border-right: 1px solid #c0ccda;
}
  .div-btn{
    margin-top: 10px;
  }
  .title{
    height: 21px;
    border-bottom: 1px solid #d3dce6;
    background-color: #f9fafc;
    padding: 10px;
    text-align: left;
  }
  .often-list>ul>li{
    list-style: none;
    float: left;
    border: 1px solid #d3dce6;
    padding: 6px;
    margin: 10px;
    background-color: #fff;
    font-size: 14px;
    cursor: pointer;
  }
  .often-price{
    color: #58b7ff;
  }
  .goods-type{
    clear: both;
  }
.cookList li{
  list-style:none;
  width: 200px;
  height: auto;
  border: 1px solid #d3dce6;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.cookList li span{
  display: block;
  text-align: left;
}
.foodImg{
  width: 35%;
  float: left;
  margin-right: 10px;
}
.foodName{
  color:brown;
  font-size: 16px;
}
.foodPrice{
  font-size: 14px;
  padding-top:10px;
}
  .goods-type{
    border-top: 1px solid #d3dce6;;
  }
  .totalDiv{
    background-color: #fff;
    padding: 10px;
    border-bottom: 1px solid #d3dce6;
  }
</style>
