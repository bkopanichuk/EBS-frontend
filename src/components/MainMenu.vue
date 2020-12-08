<template>
  <el-menu
      :default-active="activeIndex2"
      class="el-menu-demo"
      mode="horizontal"
      @select="handleSelect"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b">
    <el-menu-item index="1">Account</el-menu-item>
    <el-menu-item index="2" @click="onLogout">Logout</el-menu-item>
  </el-menu>
</template>

<script>
export default {
  name: "MainMenu",

  methods: {
    onLogout() {
      sessionStorage.clear()
      this.$emit('update')
    },
  }
}
</script>

<style scoped>

</style>