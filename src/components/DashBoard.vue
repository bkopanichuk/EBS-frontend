<template>
  <el-container>
    <el-aside width="auto">
      <SideMenu :key="sidemenu_refresh_flag" v-on:update="update($event)" v-on:update_groups="update_groups"/>
    </el-aside>
    <el-container v-if="deviceId != ''">
      <Board :key="deviceId" v-bind:deviceId = "deviceId"/>
    </el-container>
  </el-container>
</template>

<script>
import SideMenu from "@/components/SideMenu";
import Board from "@/components/Board";

export default {
  name: "DashBoard",
  components: {
    SideMenu,
    Board,
  },
  data() {
    return {
      deviceId: "",
      sidemenu_refresh_flag: false
    }
  },
  methods: {
    update(device_id) {
      this.deviceId = device_id
    },
    update_groups() {
      this.sidemenu_refresh_flag = !this.sidemenu_refresh_flag
    }
  }
}
</script>

<style scoped>

</style>