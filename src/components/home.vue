<template>
  <el-container class="wrap">
    <!-- 头部 -->
    <el-header class="header">
      <el-row :gutter="20">
        <el-col :span="3"><img src="@/assets/logo.png" alt=""></div>
        </el-col>
        <el-col :span="19">
          <div class="wrap_title">
            <h2 class="title">电商后台管理系统</h2>
          </div>
        </el-col>
        <el-col :span="1">
          <a href="" class="logout" @click='user_logout'>退出</a>
        </el-col>
      </el-row>
    </el-header>
    <el-container>

      <!-- 侧边栏 -->
      <el-aside width="200px" class="aside">
        <el-menu default-active="2" class="el-menu-vertical-demo">
          <el-submenu index="1">
            <template slot="title">
              <i class="el-icon-menu"></i> <span>用户管理</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="1-1">
                <i class="el-icon-location"></i>用户列表</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <!-- 2 -->
          <el-submenu index="2">
            <template slot="title">
              <i class="el-icon-menu"></i> <span>权限管理</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="2-1">
                <i class="el-icon-location"></i>角色列表</el-menu-item>
              <el-menu-item index="2-2">
                <i class="el-icon-location"></i>权限列表</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <!-- 3 -->
          <el-submenu index="3">
            <template slot="title">
              <i class="el-icon-menu"></i> <span>商品管理</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="3-1">
                <i class="el-icon-location"></i>商品列表</el-menu-item>
              <el-menu-item index="3-2">
                <i class="el-icon-location"></i>分类参数</el-menu-item>
              <el-menu-item index="3-3">
                <i class="el-icon-location"></i>商品分类</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <!-- 4 -->
          <el-submenu index="4">
            <template slot="title">
              <i class="el-icon-menu"></i> <span>订单管理</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="4-1">
                <i class="el-icon-location"></i>订单列表</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <!-- 5 -->
          <el-submenu index="5">
            <template slot="title">
              <i class="el-icon-menu"></i> <span>数据统计</span>
            </template>
            <el-menu-item-group>
              <el-menu-item index="5-1">
                <i class="el-icon-location"></i>数据报表</el-menu-item>
            </el-menu-item-group>
          </el-submenu>

        </el-menu>
      </el-aside>
      <!-- main部分 -->
      <el-main class="main">Main</el-main>
    </el-container>
  </el-container>
</template>
<script>
  export default {


methods: {
  user_logout(){
    localStorage.removeItem('token')
    this.$router.push({
      name:'login'
    });
    this.$message.success('退出成功')
  }
},

  }
</script>
<style>
  .wrap {
    height: 100%;
  }

  .header {
    background-color: #b3c0d1;
  }

  .aside {
    background-color: red;
  }

  .main {
    background-color: skyblue;
  }

  .title {
    line-height: 60px;
    color: white
  }

  .wrap_title {
    text-align: center
  }

  .logout {
    line-height: 60px
  }
</style>