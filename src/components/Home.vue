<template>
  <el-container>
    <!-- 头部 -->
    <el-header>
      <div class="left">
        <img src="../assets//imgs/shop.png" />
        <span>后台管理系统</span>
      </div>
      <div class="right">
        <span>欢迎您：{{ userInfo.username }}</span>
        <el-button type="danger" @click="logout">退出</el-button>
      </div>
    </el-header>
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '64px' : '200px'" >
      <div class="foldBtn" @click="isCollapse = !isCollapse">|||</div>

        <el-menu :collapse-transition="false" router unique-opened :default-active="$route.path" :collapse="isCollapse">
          <el-submenu
            :index="item.id + ''"
            v-for="item in menuList"
            :key="item.id"
          >
            <template slot="title">
              <i :class="iconObj[item.id]"></i>
              <span>{{ item.authName }}</span>
            </template>
            <el-menu-item-group>
              <el-menu-item
                :index="'/' + subItem.path"
                v-for="subItem in item.children"
                :key="subItem.id"
                >{{ subItem.authName }}</el-menu-item
              >
            </el-menu-item-group>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 主体 -->
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
      //用户信息
      userInfo: null,
      //左侧菜单列表
      menuList: [],
      iconObj: {
        201: "iconfont icon-shouye",
        125: "iconfont icon-users",
        103: "iconfont icon-tijikongjian",
        101: "iconfont icon-shangpin",
        102: "iconfont icon-danju",
      },
      //控制折叠左侧菜单
      isCollapse:false
    };
  },
  created() {
    this.userInfo = JSON.parse(sessionStorage.getItem("userInfo"));
    this.getLeftMenuList();
  },
  methods: {
    //账户登出
    logout() {
      this.$confirm("确定要退出登录吗?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          sessionStorage.clear();
          this.$router.push("./login");
        })
        .catch(() => {});
    },
    //获取左侧菜单列表
    async getLeftMenuList() {
      const { data: res } = await this.$http.get("menus");
      console.log("获取左侧菜单列表:", res);
      if (res.meta.status !== 200) {
        return this.$message.error(res.meta.msg);
      }
      this.menuList = res.data;
      console.log("this.menuListL:", this.menuList);
    },
  },
};
</script>

<style lang="less" scoped>
.el-container {
  height: 100%;
}
.el-header {
  background: url("../assets/imgs/header_bg.gif") repeat-x center;
  height: 50px !important;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: white;
  .left {
    display: flex;
    align-items: center;
    img {
      width: 40px;
      margin-right: 20px;
    }
    span {
      font-size: 20px;
    }
  }
  .right {
    span {
      margin-right: 10px;
    }
  }
}
.el-aside {
  .el-menu {
    border-right: 0;
    .iconfont {
      margin-right: 10px;
    }
  }
  .foldBtn{
    background-color: #60779D;
    display: flex;
    flex: 1;
    font-size: 12px;
    line-height: 24px;
    justify-content: center;
    align-items: center;
    color: #FFFFFF;
    letter-spacing: 0.2rem;
    cursor: pointer;
  }
}
.el-main{
  background-color: #eaedf1;

}
</style>