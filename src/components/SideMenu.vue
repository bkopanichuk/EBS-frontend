<template>
  <div style="box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1); margin: 20px">

    <el-menu>
      <div style="padding-top: 10px; padding-bottom: 10px; font-size: 24px; background: lightgray">Your Groups</div>
      <GroupOfDevices v-on:update="update($event)" v-on:update_groups="update_groups" v-for="(group, index) of groups" :key="group.id" v-bind:group="group" v-bind:index="index" />
      <div style="padding-top: 10px; padding-bottom: 10px; font-size: 24px; background: lightgray">Your Devices</div>
      <Device v-on:update="update($event)" v-on:update_groups="update_groups" v-for="(device, index) of devices" :key="device.id" v-bind:device="device" v-bind:index="index"/>
    </el-menu>
    <AddGroupForm v-on:update_groups="update_groups"/>
    <AddDeviceForm v-on:update_groups="update_groups"/>
  </div>
</template>

<script>
import GroupOfDevices from "@/components/SideMenuComponents/GroupOfDevices";
import AddGroupForm from "@/components/SideMenuComponents/AddGroupForm";
import Device from "@/components/SideMenuComponents/Device";
import AddDeviceForm from "@/components/SideMenuComponents/AddDeviceForm";

export default {
  name: "SideMenu",
  components: {
    AddDeviceForm,
    GroupOfDevices,
    AddGroupForm,
    Device
  },
  data() {
    return {
      groups: [],
      devices: []
    }
  },

  methods: {
    update_groups() {
      this.$emit('update_groups')
    },

    parseGroups() {
      fetch('https://meter-solutions.herokuapp.com/api/device_group/', {
        method: 'GET',
        headers: {
          'Authorization': 'Bearer ' + sessionStorage.getItem('token'),
        },
      })
          .then(response => response.json())
          .then(json => {
            console.log(json)
            this.groups = json
          })
    },

    parseDevices() {
      fetch('https://meter-solutions.herokuapp.com/api/device/', {
        method: 'GET',
        headers: {
          'Authorization': 'Bearer ' + sessionStorage.getItem('token'),
        },
      })
          .then(response => response.json())
          .then(json => {
            console.log(json)
            this.devices = json
          })
    },

    update(device_id) {
      this.$emit('update', device_id)
    }
  },

  mounted() {
    this.parseGroups()
    this.parseDevices()
  }
}
</script>

<style scoped>

</style>