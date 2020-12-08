<template>
  <el-submenu v-bind:index=index.toString()>
    <template slot="title"><i @click="delete_group" class="el-icon-remove-outline"></i>{{group.name}}</template>
    <Device v-on:update="update($event)" v-on:update_groups="update_groups" v-for="(device, device_index) of group.devices" :key="device.id" v-bind:device="device" v-bind:index="device_index"/>
  </el-submenu>
</template>

<script>
import Device from "@/components/SideMenuComponents/Device";

export default {
  name: "GroupOfDevices",

  props: {
    group: {
      type: Object,
    },
    index: {
      type: Number,
    }
  },

  components: {
    Device,
  },

  methods: {
    update(device_id) {
      this.$emit('update', device_id)
    },
    async delete_group() {
      await fetch('https://meter-solutions.herokuapp.com/api/device_group/' + this.group.id, {
        method: 'DELETE',
        headers: {
          'Authorization': 'Bearer ' + sessionStorage.getItem('token'),
        },
      })
      this.$emit('update_groups')
    },
    update_groups() {
      this.$emit('update_groups')
    },
  },

  computed: {

  }
}
</script>

<style scoped>

</style>