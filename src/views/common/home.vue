<template>
  <div class="mod-home">
    <h3>区块链钱包开放平台</h3>
    <ul>
      <li>1 创建应用或者游戏等产品</li>
      <li>2 根据产品ID（APPID）和产品key（AppKey）对接到游戏或者应用服务器</li>
      <li>3 测试</li>
      <li>4 正式上线</li>
      <li>联系官方获取帮助</li>
      <!-- <li>发布时，可动态配置CDN静态资源／切换新旧版本</li>
      <li>演示地址：<a href="http://fast.demo.renren.io" target="_blank">fast.demo.renren.io</a> (账号密码：admin/admin)</li> -->
    </ul>
    
    <h3>接入说明</h3>
    <ul>
      <li>
        <h3>原理</h3>
        <p>eos充值是：统一充值到钱包通过不同的memo来识别不同的用户，当用户注册的时候会生成refercode（系统唯一）。当用户充值的时候回根据用户所在的产品通过“回调地址”通知到相应的应用服务器，然后服务器对用户进行充值。</p>
      </li>
      <li>1 用户注册</li>
      <li>2 用户登录</li>
      <li>3 获取钱包信息</li>
      <li>4 提现</li>
    </ul>
    <!-- 
    <h3>官方QQ群</h3>
    <ul>
      <li>高级群：324780204(大牛云集，跟大牛学习新技能)</li>
      <li>普通群：145799952(学习交流，互相解答各种疑问)</li>
    </ul> -->
  </div>
</template>

<script>
  export default {
  }
</script>

<style>
  .mod-home {
    line-height: 1.5;
  }
</style>

