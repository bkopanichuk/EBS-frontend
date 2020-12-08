<template>
  <div id="app">
    <div v-if="is_auth()">
      <el-header>
        <MainMenu v-on:update="update()"/>
      </el-header>
      <el-main>
        <DashBoard/>
      </el-main>
    </div>
    <div v-else>
      <Login v-on:update="update()"/>
    </div>
  </div>
</template>

<script>
import MainMenu from "@/components/MainMenu";
import DashBoard from "@/components/DashBoard";
import Login from "@/components/Login";

export default {
  name: 'App',
  data() {
    return {
      test: [
        {id: 1, title: 'test1', flag: false},
        {id: 2, title: 'test2', flag: false},
        {id: 3, title: 'test3', flag: false}
      ],
    }
  },
  components: {
    MainMenu,
    DashBoard,
    Login
  },
  methods: {
    is_auth() {
      console.log(sessionStorage.getItem('token'))
      if (sessionStorage.getItem('token') == 'undefined' || sessionStorage.getItem('token') == null) {
        return false
      } else {
        return true
      }
    },

    update() {
      this.$forceUpdate();
    }
  },
}
</script>

<style>
  @import url("//unpkg.com/element-ui@2.14.1/lib/theme-chalk/index.css");

  html, body {
    margin: 0px !important;
    padding: 0px !important;
    font-family: Avenir, Helvetica, Arial, sans-serif !important;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
  }
  .el-main {
    padding: 0px;
  }
  .el-header {
    padding: 0px;
  }
    .bg-purple {
    background: black;
  }
  .bg-purple-light {
    background: red;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
</style>
