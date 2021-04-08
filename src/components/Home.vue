<template>
  <el-container>
    <!-- 侧边栏 -->
    <el-aside :width="isCollapse ? '64px' : '200px'">
      <h5>logo</h5>
      <el-menu
        background-color="#515a6e"
        text-color="#fff"
        active-text-color="pink"
        active-background-color="#4a7ee4"
        unique-opened
        :collapse="isCollapse"
        :collapse-transition="false"
        router
        :default-active="activePath"
      >
        <el-menu-item>
          <template slot="title">
            <i
              class="el-icon-s-home"
              style="color: #fff; margin-right: 16px"
            ></i>
            <span>后台首页</span>
          </template>
        </el-menu-item>
        <el-submenu
          :index="item.id + ''"
          v-for="item in menulist"
          :key="item.id"
        >
          <template slot="title">
            <i
              :class="iconsObj[item.id]"
              style="color: #fff; margin-right: 16px"
            ></i>
            <span>{{ item.authName }}</span>
          </template>
          <!-- 二级菜单 -->
          <el-menu-item
            :index="'/' + subItem.path"
            v-for="subItem in item.children"
            :key="subItem.id"
            @click="saveNavState('/' + subItem.path)"
            >{{ subItem.authName }}</el-menu-item
          >
        </el-submenu>
      </el-menu>
    </el-aside>
    <el-container>
      <!-- 头部区域 -->
      <el-header>
        <div class="title">
          <span class="toggle-button" @click="toggleCollapse">
            <i :class="isCollapse ? 'el-icon-s-unfold' : 'el-icon-s-fold'"></i>
          </span>
          <el-breadcrumb separator="/">
            <el-breadcrumb-item :to="{ path: '/home' }"
              >后台首页</el-breadcrumb-item
            >
            <el-breadcrumb-item
              ><a href="/users">用户管理</a></el-breadcrumb-item
            >
            <el-breadcrumb-item>用户列表</el-breadcrumb-item>
          </el-breadcrumb>
        </div>
        <div class="right">
          <el-menu class="el-menu-demo" mode="horizontal">
            <el-menu-item index="1"><i class="icon-search"></i></el-menu-item>
            <el-menu-item index="2"><i class="el-icon-bell"></i></el-menu-item>
            <el-menu-item>
              <el-dropdown @command="handleCommand" trigger="click">
                <span class="el-dropdown-link">
                  用户名<i class="el-icon-arrow-down el-icon--right"></i>
                </span>
                <el-dropdown-menu slot="dropdown">
                  <el-dropdown-item icon="el-icon-setting" command="info"
                    >账号信息</el-dropdown-item
                  >
                  <el-dropdown-item icon="el-icon-right" command="logout"
                    >退出登录</el-dropdown-item
                  >
                </el-dropdown-menu>
              </el-dropdown></el-menu-item
            >
          </el-menu>
        </div>
      </el-header>
      <!-- 右侧内容主体 -->
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      menulist: [],
      iconsObj: {
        125: 'el-icon-s-custom',
        103: 'el-icon-s-management',
        101: 'el-icon-shopping-cart-full',
        102: 'el-icon-bank-card',
        145: 'el-icon-data-line'
      },
      //   是否折叠
      isCollapse: false,
      //   被激活的链接地址
      activePath: ''
    }
  },
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    handleCommand(command) {
      if (command === 'info') {
        this.$message.info('还没定义')
      }
      if (command === 'logout') {
        window.sessionStorage.clear()
        this.$router.push('/login')
        this.$message.success('已退出登录')
      }
    },
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
      console.log(res)
    },
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang="scss" scoped>
.el-container {
  height: 100%;
  background-color: #f5f6f9;
  .el-aside {
    background-color: #515a6e;
    transition: all 0.5s;
    h5 {
      color: #fff;
      text-align: center;
      padding-left: 0.625rem;
      padding-right: 0.625rem;
    }
    .el-menu {
      border-right: none;
      .el-submenu {
        .el-menu-item {
          text-align: center;
          background-color: #000c17;
        }
      }
    }
  }
  .el-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #fff;
    .toggle-button {
      color: #777;
      font-size: 1.25rem;
      margin-right: 0.625rem;
      cursor: pointer;
    }
    .title {
      color: #777;
      display: flex;
      align-items: center;
    }
    .right {
      display: flex;
      align-items: center;
    }
  }
  .el-main {
  }
}
</style>
