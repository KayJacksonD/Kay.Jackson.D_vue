<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <el-row>
        <el-menu
          mode="horizontal"
          background-color="#000000"
          text-color="#fff"
          active-text-color="#ffd04b"
        >
          <el-col :span="2">
            <el-menu-item>
              <el-tooltip
                content="点击头像播放/暂停音乐"
                placement="bottom"
                effect="light"
              >
                <a @click="playVid()">
                  <img
                    id="logoID"
                    src="../assets/headSculpture.jpg"
                    alt=""
                    width="50 px"
                    height="50 px"
                  />
                  <video
                    id="video"
                    src="../assets/bgm.mp3"
                    controls="controls"
                    hidden="hidden"
                    loop="loop"
                  ></video>
                </a>
              </el-tooltip>
            </el-menu-item>
          </el-col>
          <el-col :span="4">
            <el-submenu index="1">
              <template slot="title"> Why Kay . Jackson . D ?</template>
              <el-menu-item index="1-1" @click="reloadCurrentPage()"
                >Back to HomePage</el-menu-item
              >
              <el-menu-item index="1-2">What's Flesh</el-menu-item>
              <el-menu-item index="1-3">About Jackson</el-menu-item>
            </el-submenu>
          </el-col>
          <el-col :span="3">
            <el-submenu index="2">
              <template slot="title">My WorkSpace</template>
              <a href="workFlow.html" target="right" style="color: black">
                <el-menu-item index="2-1">myItems</el-menu-item>
              </a>
              <el-submenu index="2-2">
                <template slot="title">option2</template>
                <el-menu-item index="2-2-1">option1</el-menu-item>
              </el-submenu>
            </el-submenu>
          </el-col>
          <el-col :span="2">
            <el-menu-item index="3">Message</el-menu-item>
          </el-col>
          <el-col :span="2">
            <el-menu-item index="4"
              ><a target="_blank">Account</a></el-menu-item
            >
          </el-col>
          <el-col :span="5">
            <el-menu-item>
              <el-input
                placeholder="Search"
                v-model="input3"
                class="input-with-select"
              >
                <i class="el-icon-search" slot="append"></i>
              </el-input>
            </el-menu-item>
          </el-col>
          <!-- <el-col :span="2">
            <el-menu-item>
              <a href="login.html">
                <el-button type="text" size="medium">sin in</el-button>
              </a>
            </el-menu-item>
          </el-col> -->
          <el-col :span="2">
            <el-menu-item>
              <i class="el-icon-switch-button" @click="logout"></i>
            </el-menu-item>
          </el-col>
        </el-menu>
      </el-row>
    </el-header>
    <el-container>
      <!-- 侧边栏 -->
      <el-aside>
        <el-menu
          background-color="#ffebe5"
          text-color="#000000"
          active-text-color="#409EFF"
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="true"
          router
          :default-active="activePath"
        >
          <side-bar :list="sideBarData"></side-bar>
        </el-menu>
      </el-aside>
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import sideBar from "@/components/sideBar";
export default {
  data() {
    return {
      // 左侧菜单数据
      sideBarData: [
        {
          name: "菜单1",
          index: "menu1",
          icon: "el-icon-menu",
          children: [
            {
              name: "菜单1-1",
              index: "menu1-1",
              icon: "el-icon-menu",
              children: [
                {
                  name: "菜单1-1-1",
                  index: "menu1-1-1",
                  icon: "el-icon-menu",
                  children: [],
                },
                {
                  name: "菜单1-1-2",
                  index: "menu1-1-2",
                  icon: "el-icon-menu",
                  children: [],
                },
              ],
            },
            {
              name: "菜单1-2",
              index: "menu1-2",
              icon: "el-icon-menu",
              children: [],
            },
          ],
        },
        {
          name: "菜单2",
          index: "menu2",
          icon: "el-icon-document-copy",
          children: [
            {
              name: "数据集管理2",
              index: "dataset2",
              icon: "el-icon-document-copy",
              children: [],
            },
          ],
        },
        {
          name: "菜单2",
          index: "menu3",
          icon: "el-icon-folder",
          children: [],
        },
      ],
      headSculpture:"../assets/headSculpture.jpg", // // 该属性暂没调试通过
      ifAutoplay: true,
      // 是否折叠
      isCollapse: false,
      // 被激活的链接地址
      activePath: "",
    };
  },
  components: {
    "side-bar": sideBar,
  },
  created() {
    // this.getMenuList();
    // this.activePath = window.sessionStorage.getItem("activePath");
  },
  methods: {
    playVid() {
      let vo = document.getElementById("video");
      if (this.ifAutoplay == true) {
        vo.play();
      } else {
        vo.pause();
      }
      this.ifAutoplay = !this.ifAutoplay;
    },
    logout() {
      window.sessionStorage.clear();
      this.$router.push("/login");
    },
    // 获取所有的菜单
    async getMenuList() {
      const { data: res } = await this.$http.get("menus");
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg);
      this.menulist = res.data;
      console.log(res);
    },
    // 点击按钮，切换菜单的折叠与展开
    toggleCollapse() {
      this.isCollapse = !this.isCollapse;
    },
    // 保存链接的激活状态
    saveNavState(activePath) {
      window.sessionStorage.setItem("activePath", activePath);
      this.activePath = activePath;
    },
  },
};
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  background-color: #000000;
  // display: flex;
  // justify-content: space-between;
  // padding-left: 0;
  // align-items: center;
  // color: #fff;
  // font-size: 20px;
  // > div {
  //   display: flex;
  //   align-items: center;
  //   span {
  //     margin-left: 15px;
  //   }
  // }
}

.el-aside {
  background-color: #f7e8ba;
  .el-menu {
    border-right: none;
  }
}

.el-main {
  background-color: #e9eef3;
}

.iconfont {
  margin-right: 10px;
}

.toggle-button {
  background-color: #9ca8d1;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}

#logoID {
  border-radius: 25px;
  -webkit-border-radius: 25px;
  -moz-border-radius: 25px;
}
</style>
