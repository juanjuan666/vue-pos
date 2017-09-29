<template>
  <div class="left-nav">
    <ul>
      <li>
        <i class="icon iconfont icon-goumai"></i>
        <span>收银</span>
      </li>
      <li>
        <i class="icon iconfont icon-dianpu"></i>
        <span>店铺</span>
      </li>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <span>商品</span>
      </li>
      <li>
        <i class="icon iconfont icon-huiyuanqia"></i>
        <span>会员</span>
      </li>
      <li>
        <i class="icon iconfont icon-zuanshi"></i>
        <span>统计</span>
      </li>
      <li>
        <i class="icon iconfont icon-gongnengjianyi"></i>
        <span>设置</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.left-nav{
  color: #fff;
  font-size: 10px;
  height: 100%;
  background: #1d8cf0;
  float: left;
  width: 5%;
}
  .iconfont{
    font-size: 24px;
  }
  .left-nav>ul{
    padding: 0;
    margin: 0;
  }
  .left-nav>ul>li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #20a0ff;
    padding: 10px;
  }
  .left-nav>ul>li>span{
    display: block;
  }
</style>
