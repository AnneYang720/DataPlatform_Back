<template>
  <div class="regis-container">
    <el-form :model="registerForm" :rules="registerRules" ref="registerForm" label-position="left" label-width="0px"
      class="card-box login-form">
      <h3 class="title">数据库平台注册</h3>

      <el-form-item prop="username">
        <span class="svg-container svg-container_login">
          <svg-icon icon-class="user" />
        </span>
        <el-input name="username" type="text" v-model="registerForm.username" placeholder="姓名" />
      </el-form-item>

      <el-form-item prop="password">
        <span class="svg-container">
          <svg-icon icon-class="password"></svg-icon>
        </span>
        <el-input name="password" :type="pwdType" v-model="registerForm.password" 
          placeholder="密码"></el-input>
          <span class="show-pwd" @click="showPwd"><svg-icon icon-class="eye" /></span>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" style="width:100%;" :loading="loading" @click.native.prevent="handleRegister">
          注册
        </el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import {register} from '@/api/login'
import * as coreJS from '../../utils/core'

export default {
  name: 'register',
  data() {
    var validatePass = (rule, value, callback) => {
        var pattern = /^\S{6,20}$/g
        if (value === '') {
          callback(new Error('请输入密码'))
        } else if (!pattern.test(value)) {
          callback(new Error('请输入6-20个非空白字符'))
        } else {
          callback()
        }
    }
    return{
      registerForm: {
        username: '',
        password: ''
      },
      registerRules: {
        username: [{ required: true, message: '请输入姓名', trigger: 'blur' },],
        password: [{ required: true, trigger: 'blur', validator: validatePass }]
      },
      loading: false,
      pwdType: 'password'
    }
  },
  methods: {
    showPwd() {
      if (this.pwdType === 'password') {this.pwdType = ''} 
      else {this.pwdType = 'password'}
    },
    handleRegister() {
      this.$refs.registerForm.validate(valid => {
        if(valid){
          register(this.registerForm).then( response =>{
            if(response.Flag){
              this.$message.success("注册成功")
              this.$router.push('/login')
            } else{
              this.$message.error(response.message)
            }
          }).catch(err => {
            console.log(err)
          })
        }
      })
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss">
  $bg:#2d3a4b;
  $dark_gray:#889aa4;
  $light_gray:#eee;

  .regis-container {
    position: fixed;
    height: 100%;
    width:100%;
    background-color: $bg;
    input:-webkit-autofill {
      -webkit-box-shadow: 0 0 0px 1000px #293444 inset !important;
      -webkit-text-fill-color: #fff !important;
    }
    input {
      background: transparent;
      border: 0px;
      -webkit-appearance: none;
      border-radius: 0px;
      padding: 12px 5px 12px 15px;
      color: $light_gray;
      height: 47px;
    }
    .el-input {
      display: inline-block;
      height: 47px;
      width: 85%;
    }
    .tips {
      font-size: 14px;
      color: #fff;
      margin-bottom: 10px;
    }
    .svg-container {
      padding: 6px 5px 6px 15px;
      color: $dark_gray;
      vertical-align: middle;
      width: 30px;
      display: inline-block;
      &_login {
        font-size: 20px;
      }
    }
    .title {
      font-size: 24px;
      font-weight: 400;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
    .login-form {
      position: absolute;
      left: 0;
      right: 0;
      width: 400px;
      padding: 35px 35px 15px 35px;
      margin: 120px auto;
    }
    .el-form-item {
      border: 1px solid rgba(255, 255, 255, 0.1);
      background: rgba(0, 0, 0, 0.1);
      border-radius: 5px;
      color: #454545;
    }
    .show-pwd {
      position: absolute;
      right: 10px;
      top: 7px;
      font-size: 16px;
      color: $dark_gray;
      cursor: pointer;
      user-select:none;
    }
    .thirdparty-button{
      position: absolute;
      right: 35px;
      bottom: 28px;
    }
  }
</style>
