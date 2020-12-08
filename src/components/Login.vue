<template>
  <el-row>
    <el-col :span="12" style="padding: 20px; box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1); margin: 20px">
      <p style="font-size: 32px; padding-bottom: 20px"><b>Login</b></p>
      <el-form ref="form" :model="form" label-width="120px" >
        <el-form-item label="E-mail">
          <el-input type="email" v-model="user.email"></el-input>
        </el-form-item>
        <el-form-item label="Password">
          <el-input type="password" v-model="user.password"></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="onSubmit">Login</el-button>
        </el-form-item>
      </el-form>
    </el-col>
    <el-col :span="12" style="padding: 20px; box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1); margin: 20px">
      <p style="font-size: 32px; padding-bottom: 20px"><b>Registration</b></p>
      <el-form ref="reg_form" :model="form" label-width="120px" >
        <el-form-item label="E-mail">
          <el-input type="email" v-model="reg_user.email"></el-input>
        </el-form-item>
        <el-form-item label="Username">
          <el-input v-model="reg_user.username"></el-input>
        </el-form-item>
        <el-form-item label="Password">
          <el-input type="password" v-model="reg_user.password"></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="onSubmitReg">Register</el-button>
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>
</template>

<script>
export default {
  name: "Login",

  data() {
    return {
      user: {
        email: "",
        password: ""
      },
      reg_user: {
        email: "",
        password: "",
        username: ""
      },
    }
  },

  methods: {
    onSubmit() {
      fetch('https://meter-solutions.herokuapp.com/login/', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json;charset=utf-8'
        },
        body: JSON.stringify(this.user)
      })
          .then(response => response.json())
          .then(json => {

            sessionStorage.setItem('token', json.token)
            console.log(sessionStorage.getItem('token'))
            this.parseUser()
          })
      this.user.password = ''
    },
    onSubmitReg() {
      fetch('https://meter-solutions.herokuapp.com/registration/', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json;charset=utf-8'
        },
        body: JSON.stringify(this.reg_user)
      })
      this.reg_user.email = ''
      this.reg_user.password = ''
      this.reg_user.username = ''
    },
    parseUser() {
      fetch('https://meter-solutions.herokuapp.com/get_user_by_mail/', {
        method: 'POST',
        headers: {
          'Authorization': 'Bearer ' + sessionStorage.getItem('token'),
          'Content-Type': 'application/json;charset=utf-8'
        },
        body: JSON.stringify(this.user)
      })
          .then(response => response.json())
          .then(json => {
            sessionStorage.setItem('user_id', json.id)
            sessionStorage.setItem('user_email', json.email)
            sessionStorage.setItem('user_username', json.username)
          })
      this.user.email = ''
      this.$emit('update')
    }
  }
}
</script>

<style scoped>

</style>