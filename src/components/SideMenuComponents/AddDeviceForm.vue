<template>
  <el-form :model="addDeviceForm" status-icon :rules="rules" ref="addDeviceForm" label-width="auto" class="form">
    <el-row>
      <el-col :span="24">
        <el-form-item style="margin: 20px" label="Add device" prop="name">
          <el-input style="padding-bottom: 20px" v-model="addDeviceForm.name" placeholder="Name of device"></el-input>
          <el-input v-model="addDeviceForm.mqtt_id" placeholder="MQTT ID of device"></el-input>
        </el-form-item>
        <el-button type="primary" class="submitButton"  @click="submitForm('addDeviceForm')">Add</el-button>
      </el-col>
    </el-row>
  </el-form>
</template>

<script>
export default {
  name: "AddDeviceForm",

  data() {
    var validateName = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('Please input the name again'));
      } else if (value.length > 20) {
        callback(new Error('Name is too long'));
      } else {
        callback();
      }
    };
    return {
      addDeviceForm: {
        name: '',
        mqtt_id: "",
        //TODO parse device models
        device_model: "1"
      },
      rules: {
        name: [
          { validator: validateName, trigger: 'blur' }
        ],
      }
    }
  },

  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.addDevice()
          this.$refs[formName].resetFields();
          this.$emit('update_groups')
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    async addDevice() {
      await fetch('https://meter-solutions.herokuapp.com/api/device/', {
        method: 'POST',
        headers: {
          'Authorization': 'Bearer ' + sessionStorage.getItem('token'),
          'Content-Type': 'application/json;charset=utf-8'
        },
        body: JSON.stringify(this.addDeviceForm)
      })
    }
  }

}
</script>

<style scoped>
.form {
  padding-left: 10px;
  display: flex;
  background: lightblue;
  padding-top: 43px;
  padding-bottom: 20px;
  bottom: 0px;
}
.submitButton {
  width: 90%;
}
</style>