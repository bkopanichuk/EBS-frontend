<template>
  <el-menu-item-group>
    <el-menu-item @click="selectDevice" v-bind:index="index"><i @click="delete_device" class="el-icon-remove-outline"></i>{{device.name}} : {{device.device_model.name}}</el-menu-item>
  </el-menu-item-group>
</template>

<script>
export default {
  name: "Device",

  props: {
    device: {
      type: Object,
    },
    index: {
      type: Number,
    },
  },

  methods: {
    selectDevice() {
      this.$emit('update', this.device.id)
    },
    async delete_device() {
      await fetch('https://meter-solutions.herokuapp.com/api/device/' + this.device.id, {
        method: 'DELETE',
        headers: {
          'Authorization': 'Bearer ' + sessionStorage.getItem('token'),
        },
      })
      this.$emit('update_groups')
    }
  }
}
</script>

<style scoped>

</style>